mutt-ical
=========

This is a collection of scripts that make it easier to work with
[iCalendar][] files in [mutt][].  (Note that this is for calendar
information in the iCalendar *file format*.  It has nothing to do with the
OSX calendar program.)

  [iCalendar]: https://en.wikipedia.org/wiki/iCalendar
  [mutt]: http://www.mutt.org


viewical
--------

`viewical` takes an iCalendar file on standard input and prints out a more
human-friendly rendering of the data in the file.  It's intended to be
used as a display filter in mutt.

### Requirements

* Python
* [python-icalendar](http://icalendar.readthedocs.org/)
* [pytz](http://pythonhosted.org/pytz/)
* [tzlocal](https://github.com/regebro/tzlocal)

### Usage

This is easiest if you maintain a mutt-specific mailcap, e.g. having this
in your `~/.muttrc`:

    set mailcap_path="${HOME}/.mutt/mailcap:/etc/mailcap"

In your mailcap, add entries for the appropriate MIME types:

    text/calendar; /path/to/viewical; copiousoutput
    application/ics; /path/to/viewical; copiousoutput

In your `.muttrc`, tell mutt to automatically display calendar data:

    auto_view text/calendar
    auto_view application/ics

Finally, you need to add (or modify) the `alternative_order` setting in
your `.muttrc` to prefer iCalendar attachments over their HTML or text
alternatives, for messages sent with such alternatives:

    alternative_order text/calendar text/plain text/html

### Output

Most of the script's output should be self-explanatory.  Most fields are
optional, so it'll only print information (from event end times to
locations to event descriptions) if they're present in the original data.

One thing to note is the encoding of attendees (or, in iCalendar
terminology, "participants").  They're presented in a list with a checkbox
of sorts next to them, something like this:

    [ ] Barb Example <barb@example.com>

People will get different boxes depending on the role defined for them in
the iCalendar data.  The boxes are as follows:

* `{ }` - Event chairperson.
* `[ ]` - Attendee, participation required.  (Most programs use this as
          the default role.)
* `< >` - Attendee, participation optional.
* `( )` - Non-participant.  (The author of these scripts has never seen
          this in actual use.)
* `_ _` - No role defined in the data.
* `? ?` - Unknown role.

The script places text in the box to indicate the status of the person.
The statuses are as follows:

* blank - Unknown.  (Officially, this is "needs action", i.e. "waiting for
          a response".)
* `Y` - Attending.
* `-` - Not attending.
* `~` - Maybe attending.
* `?` - Status not recognized by script.

(In the event that the iCalendar data does not define a status, the box
will be empty, not just blank.  This is "status unknown to organizer":
`[ ]`.  This is "status not present in data": `[]`.  That's not a huge
difference, but every file the script's author has observed has had some
status defined for every person attached to an event.)

#### Example

Here's an event with a chairperson, two required attendees, and two
non-required attendees.  The chairperson and one required attendee have
responded that they will attend.  The other required attendee has not yet
responded.  One of the non-required attendees will not attend and the
other is tentative.

    Organizer: Admin Aid <admin@example.com>
    Event:     Example Event
    Date:      Thursday, August 4, 2016
    Starts:    9:00 am
    Ends:      10:00 am
    Location:  Meeting Room 7
    Attendees: {Y} Important Executive <exec@example.com>
               [Y] Relevant Manager <mgr@example.com>
               [ ] Relevant Subordinate <worker@example.com>
               <-> Affiliated Manager <aff@example.com>
               <~> Irrelevant Manager <irr@example.com>


ical-reply
----------

`ical-reply` is intended to facilitate responses to iCalendar emails.
It's not ready for use yet.
