---
layout: post
title: 关于Ruby的时间
categories: [Tech]
tags: [ruby]
---

### TODO：主要内容

* UTC & GMT
* Time
* Datetime
* timecop
* Timecop.freeze(24.hours.from_now)
* Timezone gem?

ActiveSupport::TimeZone.new("").formatted_offset
                               .local(*args)

now.in_time_zone('')

now.change

gem: TZInfo

ActiveSupport::TimeZone
The TimeZone class serves as a wrapper around TZInfo::Timezone instances.

1. Limit the set of zones provided by TZInfo to a meaningful subset of 146 zones
2. Retrieve and display zones with a friendlier name
3. Lazily load TZInfo::Timezone instances only when they're needed.
4. Create ActiveSupport::TimeWithZone instances via TimeZone's local, parse, at and now methods.

set config.time_zone in the Rails Application, you can access this TimeZone object via Time.zone:

# application.rb:
class Application < Rails::Application
  config.time_zone = 'Eastern Time (US & Canada)'
end

Time.zone      # => #<ActiveSupport::TimeZone:0x514834...>
Time.zone.name # => "Eastern Time (US & Canada)"
Time.zone.now  # => Sun, 18 May 2008 14:30:44 EDT -04:00
Time.zone.today

### in\_time\_zone
You shouldn't ever need to create a TimeWithZone instance directly via new. Instead use methods local, parse, at and now on TimeZone instances, and in_time_zone on Time and DateTime instances.

Time.zone = 'Hawaii'        # => 'Hawaii'
DateTime.utc(2000).in_time_zone # => Fri, 31 Dec 1999 14:00:00 HST -10:00
Date.new(2000).in_time_zone  # => Sat, 01 Jan 2000 00:00:00 HST -10:00

Time.utc(2000).in_time_zone('Alaska') # => Fri, 31 Dec 1999 15:00:00 AKST -09:00
DateTime.utc(2000).in_time_zone('Alaska') # => Fri, 31 Dec 1999 15:00:00 AKST -09:00
Date.new(2000).in_time_zone('Alaska')  # => Sat, 01 Jan 2000 00:00:00 AKST -09:00

### see all the available time zones, run:
rake time:zones:all


The default time zone in Rails is UTC.

### Apply user's preferred time zone:

```
# app/controllers/application_controller.rb
around_action :set_time_zone, if: :current_user

private

def set_time_zone(&block)
  Time.use_zone(current_user.time_zone, &block)
end
```

display times in a specific user’s time zone, we can use Time’s `in_time_zone` method:

## Working with APIs
When working with APIs, it is best to use the ISO8601 standard, which represents date/time information as a string. ISO8601’s advantages are that the string is unambiguous, human readable, widely supported, and sortable.

```
> timestamp = Time.now.utc.iso8601
=> "2015-07-04T21:53:23Z"
```

The Z at the end of the string indicates that this time is in UTC, not a local time zone. To convert the string back to a Time instance, we can say:

```
> Time.iso8601(timestamp)
=> 2015-07-04 21:53:23 UTC
```

```
# This is the time on my machine, also commonly described as "system time"
> Time.now
=> 2015-07-04 17:53:23 -0400

# Let's set the time zone to be Fiji
> Time.zone = "Fiji"
=> "Fiji"

# But we still get my system time
> Time.now
=> 2015-07-04 17:53:37 -0400

# However, if we use `zone` first, we finally get the current time in Fiji
> Time.zone.now
=> Sun, 05 Jul 2015 09:53:42 FJT +12:00

# We can also use `current` to get the same
> Time.current
=> Sun, 05 Jul 2015 09:54:17 FJT +12:00

# Or even translate the system time to application time with `in_time_zone`
> Time.now.in_time_zone
=> Sun, 05 Jul 2015 09:56:57 FJT +12:00

# Let's do the same with Date (we are still in Fiji time, remember?)
# This again is the date on my machine, system date
> Date.today
=> Sat, 04 Jul 2015

# But going through `zone` again, and we are back to application time
> Time.zone.today
=> Sun, 05 Jul 2015

# And gives us the correct tomorrow according to our application's time zone
> Time.zone.tomorrow
=> Mon, 06 Jul 2015

# Going through Rails' helpers, we get the correct tomorrow as well
> 1.day.from_now
=> Mon, 06 Jul 2015 10:00:56 FJT +12:00
```

### Time zone related querying
Rails saves timestamps to the database in UTC time zone. We should always use Time.current for any database queries, so that Rails will translate and compare the correct times.

```
Post.where("published_at > ?", Time.current)
# SELECT "posts".* FROM "posts" WHERE (published_at > '2015-07-04 17:45:01.452465')
```

### A summary of do’s and don'ts with time zones
**DON’T USE**

```
* Time.now
* Date.today
* Date.today.to_time
* Time.parse("2015-07-04 17:05:37")
* Time.strptime(string, "%Y-%m-%dT%H:%M:%S%z")
```

**DO USE**

```
* Time.current
* 2.hours.ago
* Time.zone.today
* Date.current
* 1.day.from_now
* Time.zone.parse("2015-07-04 17:05:37")
* Time.strptime(string, "%Y-%m-%dT%H:%M:%S%z").in_time_zone
```

### Testing time zones
Rails 4.1 added a ActiveSupport::Testing::TimeHelpers module, with three useful methods: travel, travel_back, and travel_to. We can use these methods to freeze time within blocks in our tests.

If using an older version of Rails, there are three gems to help us set and freeze the time in our tests: Timecop, Delorean, and Zonebie. As much as I love the reference to go back_to_the_present, I usually use Timecop.

```
new_time = Time.zone.parse("2014-10-19 1:00:00")

# With Timecop, we can freeze the time,
Timecop.freeze new_time

# or
Timecop.travel new_time

# but will need to clean up after the spec, and return to current time
Timecop.return

# Alternatively we can use blocks, which only freeze the time inside our block
Time.use_zone("Sydney") do
end

# With Delorean, the syntax is a touch different
Delorean.time_travel_to("1 month ago") do
end
Delorean.back_to_the_present

# And Zonebie sets the time zone to a random one each time we run our tests
Zonebie.set_random_timezone
```

## In summary
* Always work with UTC.
* Use Time.current or Time.zone.today.
* Use testing helper methods of your choice to freeze the time in your tests, preferably by using a block.


## 参考
https://robots.thoughtbot.com/its-about-time-zones
