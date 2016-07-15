mutt-ical
=========

This is a collection of scripts that make it easier to work with
[iCalendar][] files in [mutt][].  (Note that its is for calendar
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
    

ical-reply
----------

`ical-reply` is intended to facilitate responses to iCalendar emails.
It's not ready for use yet.
