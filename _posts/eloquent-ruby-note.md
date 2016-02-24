
##Chapter 5 Regular Expressions

two periods ( .. ) will match any two characters

\d will match any digit
\w, where the w stands for “word character,” will match any letter, number or the underscore.
\s will match any white space character including the vanilla space, the tab, and the newline.


turn that case sensitivity off my sticking an i on the end of your expression /AM/i =~ 'am'

\A matches the unseen leading edge of the string
\z (note the lower case) matches the end of the string

The circumflex character matches two things: the beginning of the string or the beginning of any line within the string.
the dollar sign $ matches the end of the string or the end of any line within the string

? the question mark matches zero or one of the things that came before

##Chapter 6 Use Symbols to Stand for Something

HashWithIndifferentAccess class is a subclass of Hash that allows you to mix and match strings and symbols with cheerful abandon.

symbols are specially tuned to their “stands for” purpose: Symbols are both unique—there can only ever be one :all symbol in your Ruby interpreter—and immutable, so that :all will never change.

##Chapter 7 Treat Everything Like an Object—Because Everything Is

classes act as containers for methods
classes are also factories, factories for making instances:

true.class         # Returns Trueclass
nil.class            # Returns NilClass

Reflection-oriented methods

the public_methods method, which returns an array of all the method names available on the object

public—callable by any code anywhere,
in Ruby, private methods are callable from subclasses

Any instance of a class can call a protected method on any other instance of the class. Thus, if we made word_count protected, any instance of Document could call word_count on any other instance of Document, including instances of subclasses like RomanceNovel.

The Ruby philosophy is that the programmer is in charge.

##

##
