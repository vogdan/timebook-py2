.. -*- restructuredtext -*-

Timebook
========

Timebook is a small utility which aims to be a low-overhead way of
tracking what you spend time on. It can be used to prepare annotated
time logs of work for presentation to a client, or simply track how you
spend your free time. Timebook is implemented as a python script which
maintains its state in a sqlite3 database.

Concepts
~~~~~~~~

Timebook maintains a list of *timesheets* -- distinct lists of timed
*periods*. Each period has a start and end time, with the exception of the
most recent period, which may have no end time set. This indicates that
this period is still running. Timesheets containing such periods are
considered *active*. It is possible to have multiple timesheets active
simultaneously, though a single time sheet may only have one period
running at once.

Interactions with timebook are performed through the ``t`` command on
the command line. ``t`` is followed by one of timebook's subcommands.
Often used subcommands include ``in``, ``out``, ``switch``, ``now``,
``list`` and ``display``. Commands may be abbreviated as long as they
are unambiguous: thus ``t switch foo`` and ``t s foo`` are identical.
With the default command set, no two commands share the first same
letter, thus it is only necessary to type the first letter of a command.
Likewise, commands which display timesheets accept abbreviated timesheet
names. ``t display f`` is thus equivalent to ``t display foo`` if
``foo`` is the only timesheet which begins with "f". Note that this does
not apply to ``t switch``, since this command also creates timesheets.
(Using the earlier example, if ``t switch f`` is entered, it would thus
be ambiguous whether a new timesheet ``f`` or switching to the existing
timesheet ``foo`` was desired).

Usage
~~~~~

The basic usage is as follows::

  $ t [OPTIONS] COMMAND [ARGS...]
  
  
Usage examples::

  $ t s writing
  $ t i document timebook
  $ t o

and their equivalents::

  $ t switch writing
  $ t in document timebook
  $ t out

The first command, ``t switch writing``, switches to the timesheet
"writing" (or creates it if it does not exist). ``t in document
timebook`` creates a new period in the current timesheet, and annotates
it with the description "document timebook". Note that this command
would be in error if the ``writing`` timesheet was already active.
Finally, ``t out`` records the current time as the end time for the
most recent period in the ``writing`` timesheet.

To display the current timesheet, invoke the ``t d`` or ``t display`` command::

  $ t display
  Timesheet writing:
  Day            Start      End        Duration   Notes
  Mar 14, 2009   19:53:30 - 20:06:15   0:12:45    document timebook
                 20:07:02 -            0:00:01    write home about timebook
                                       0:12:46
  Total                                0:12:46

Each period in the timesheet is listed on a row. If the timesheet is
active, the final period in the timesheet will have no end time. After
each day, the total time tracked in the timesheet for that day is
listed. Note that this is computed by summing the durations of the
periods beginning in the day. In the last row, the total time tracked in
the timesheet is shown.

Commands
~~~~~~~~

To get info about available commands and options use::

  $ t -h
  
All commands have a ``-h`` option that shows specific command help::

  $ t d -h

will show help for ``display`` or ``d`` command 
