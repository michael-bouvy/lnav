
.. _commands:

Command Reference
=================

This reference covers the commands used to control **lnav**.  Consult the
`built-in help <https://github.com/tstack/lnav/blob/master/src/help.txt>`_ in
**lnav** for a more detailed explanation of each command.

Note that almost all commands support TAB-completion for their arguments, so
if you are in doubt as to what to type for an argument, you can double tap the
TAB key to get suggestions.

Filtering
---------

The set of log messages that are displayed in the log view can be controlled
with the following commands:

* filter-in <regex> - Only display log lines that match a regex.
* filter-out <regex> - Do not display log lines that match a regex.
* disable-filter <regex> - Disable the given filter.
* enable-filter <regex> - Enable the given filter.
* delete-filter <regex> - Delete the filter.
* set-min-log-level <level> - Only display log lines with the given log level
  or higher.
* hide-lines-before <abs-time|rel-time> - Hide lines before the given time.
* hide-lines-after <abs-time|rel-time> - Hide lines after the given time.
* show-lines-before-and-after - Show lines that were hidden by the "hide-lines" commands.

Navigation
----------

* goto <line#|N%|abs-time|relative-time> - Go to the given line number, N
  percent into the file, the given timestamp in the log view, or by the
  relative time (e.g. 'a minute ago').
* relative-goto <line#|N%> - Move the current view up or down by the given
  amount.
* mark - Bookmark the top line in the view.
* next-mark error|warning|search|user|file|partition - Move to the next
  bookmark of the given type in the current view.
* prev-mark error|warning|search|user|file|partition - Move to the previous
  bookmark of the given type in the current view.

Time
----

* adjust-log-time <date> - Change the timestamps for a log file.
* unix-time <secs-or-date> - Convert a unix-timestamp in seconds to a
  human-readable form or vice-versa.
* current-time - Print the current time in human-readable form and as
  a unix-timestamp.

Display
-------

* help - Display the built-in help text.

* disable-word-wrap - Disable word wrapping in the log and text file views.
* enable-word-wrap - Enable word wrapping in the log and text file views.

* hide-fields <field-name> [<field-name2> ... <field-nameN>] - Hide large log
  message fields by replacing them with an ellipsis.  You can quickly switching
  between showing and hiding hidden fields using the 'x' hotkey.

* show-fields <field-name> [<field-name2> ... <field-nameN>] - Show previously
  hidden log message fields.

* highlight <regex> - Colorize text that matches the given regex.
* clear-highlight <regex> - Clear a previous highlight.

* spectrogram <numeric-field> - Generate a spectrogram for a numeric log
  message field or SQL result column. The spectrogram view displays the range
  of possible values of the field on the horizontal axis and time on the
  vertical axis.  The horizontal axis is split into buckets where each bucket
  counts how many log messages contained the field with a value in that range.
  The buckets are colored based on the count in the bucket: green means low,
  yellow means medium, and red means high.  The exact ranges for the colors is
  computed automatically and displayed in the middle of the top line of the
  view.  The minimum and maximum values for the field are displayed in the
  top left and right sides of the view, respectively.

* switch-to-view <name> - Switch to the given view name (e.g. log, text, ...)

* zoom-to <zoom-level> - Set the zoom level for the histogram view.

* redraw - Redraw the window to correct any corruption.

* alt-msg <msg> - Set the message to be displayed on the bottom-right of the
  screen.  This message is typically used for help text.


SQL
---

* create-logline-table <table-name> - Create an SQL table using the top line
  of the log view as a template.  See the :ref:`data-ext` section for more information.

* delete-logline-table <table-name> - Delete a table created by create-logline-table.

* create-search-table <table-name> [regex] - Create an SQL table that
  extracts information from logs using the provided regular expression or the
  last search that was done.  Any captures in the expression will be used as
  columns in the SQL table.  If the capture is named, that name will be used as
  the column name, otherwise the column name will be of the form 'col_N'.
* delete-search-table <table-name> - Delete a table that was created with create-search-table.


Output
------

* append-to <file> - Append any bookmarked lines in the current view to the
  given file.
* write-to <file> - Overwrite the given file with any bookmarked lines in
  the current view.  Use '-' to write the lines to the terminal.
* write-csv-to <file> - Write SQL query results to the given file in CSV format.
  Use '-' to write the lines to the terminal.
* write-json-to <file> - Write SQL query results to the given file in JSON
  format.  Use '-' to write the lines to the terminal.
* pipe-to <shell-cmd> - Pipe the bookmarked lines in the current view to a
  shell command and open the output in lnav.
* pipe-line-to <shell-cmd> - Pipe the top line in the current view to a shell
  command and open the output in lnav.

.. _misc-cmd:

Miscellaneous
-------------

* echo [-n] <msg> - Display the given message in the command prompt.  Useful
  for scripts to display messages to the user.  The '-n' option leaves out the
  new line at the end of the message.
* eval <cmd> - Evaluate the given command or SQL query after performing
  environment variable substitution.  The argument to *eval* must start with a
  colon, semi-colon, or pipe character to signify whether the argument is a
  command, SQL query, or a script to be executed, respectively.

Configuration
-------------

* config <option> - Get the current value of a configuration option.
* config <option> <value> - Set the value of a configuration option.
* reset-config <option> - Reset a configuration option to the default.
* save-config - Save the current configuration to ~/.lnav/config.json.

The following options are available:

* /ui/clock-format - Specifies the date-time format of the clock in the
  top-left corner of the UI.  The format conversion specifiers are the same as
  in strftime(3).
* /ui/dim-text - Reduce the brightness of text.  This setting can be useful
  when running in an xterm where the white color is very bright.

.. note:: The following commands can be disabled by setting the ``LNAVSECURE``
   environment variable before executing the **lnav** binary:

   - open
   - pipe-to
   - pipe-line-to
   - write-\*-to

   This makes it easier to run lnav in restricted environments without the risk
   of privilege escalation.
