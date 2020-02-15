Bash tricks
===========

Displaying the current git branch in the PS1
--------------------------------------------

Define the following command in your ``.bashrc``:

.. code-block:: bash

    parse_git_branch() {
        output=$(git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ \1/')
        if [ "$output" != "" ]; then
            echo -e "\e[1;33m${output:1}\e[1;37m"
        else
            echo " "
        fi
    }

Screen tips
===========

All the code shown below must be placed inside a ``.screenrc`` file inside your home folder.

Turn off the startup message
----------------------------

::

   startup_message off

Customize the status line
-------------------------

Here's a good article that covers the ``hardstatus string`` format : `kilobitspersecond.com <https://www.kilobitspersecond.com/2014/02/10/understanding-gnu-screens-hardstatus-strings/>`_

::

 hardstatus alwayslastline
 hardstatus string '%{= kG}[ %{G}%H %{g}][%= %{= kw}%?%-Lw%?%{r}(%{W}%n*%f%t%?(%u)%?%{r})%{w}%?%+Lw%?%?%= %{g}][%{B} %m-%d %{W}%c %{g}]'
