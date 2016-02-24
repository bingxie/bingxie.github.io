
# many to many
rails g model Attendance user:references event:references num_guests:integer
