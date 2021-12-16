# bash-alaises
my aliases for git bash 
enjoy

# To the extent possible under law, the author(s) have dedicated all 
# copyright and related and neighboring rights to this software to the 
# public domain worldwide. This software is distributed without any warranty. 
# You should have received a copy of the CC0 Public Domain Dedication along 
# with this software. 
# If not, see <https://creativecommons.org/publicdomain/zero/1.0/>. 

# /etc/bash.bashrc: executed by bash(1) for interactive shells.

# System-wide bashrc file

# Check that we haven't already been sourced.
([[ -z ${CYG_SYS_BASHRC} ]] && CYG_SYS_BASHRC="1") || return

# If not running interactively, don't do anything
[[ "$-" != *i* ]] && return

# If started from sshd, make sure profile is sourced
if [[ -n "$SSH_CONNECTION" ]] && [[ "$PATH" != *:/usr/bin* ]]; then
    source /etc/profile
fi

# Warnings
unset _warning_found
for _warning_prefix in '' ${MINGW_PREFIX}; do
    for _warning_file in ${_warning_prefix}/etc/profile.d/*.warning{.once,}; do
        test -f "${_warning_file}" || continue
        _warning="$(command sed 's/^/\t\t/' "${_warning_file}" 2>/dev/null)"
        if test -n "${_warning}"; then
            if test -z "${_warning_found}"; then
                _warning_found='true'
                echo
            fi
            if test -t 1
                then printf "\t\e[1;33mwarning:\e[0m\n${_warning}\n\n"
                else printf "\twarning:\n${_warning}\n\n"
            fi
        fi
        [[ "${_warning_file}" = *.once ]] && rm -f "${_warning_file}"
    done
done
unset _warning_found
unset _warning_prefix
unset _warning_file
unset _warning

# If MSYS2_PS1 is set, use that as default PS1;
# if a PS1 is already set and exported, use that;
# otherwise set a default prompt
# of user@host, MSYSTEM variable, and current_directory
[[ -n "${MSYS2_PS1}" ]] && export PS1="${MSYS2_PS1}"
# if we have the "High Mandatory Level" group, it means we're elevated
#if [[ -n "$(command -v getent)" ]] && id -G | grep -q "$(getent -w group 'S-1-16-12288' | cut -d: -f2)"
#  then _ps1_symbol='\[\e[1m\]#\[\e[0m\]'
#  else _ps1_symbol='\$'
#fi
[[ $(declare -p PS1 2>/dev/null | cut -c 1-11) = 'declare -x ' ]] || \
  export PS1='\[\e]0;\w\a\]\n\[\e[32m\]\u@\h \[\e[35m\]$MSYSTEM\[\e[0m\] \[\e[33m\]\w\[\e[0m\]\n'"${_ps1_symbol}"' '
unset _ps1_symbol

# Uncomment to use the terminal colours set in DIR_COLORS
# eval "$(dircolors -b /etc/DIR_COLORS)"

# Fixup git-bash in non login env
shopt -q login_shell || . /etc/profile.d/git-prompt.sh


#aliases
alias pa='php artisan'
alias pas='php artisan serve'
alias pam='php artisan migrate'
alias pamf='php artisan migrate:fresh'
alias pamm='php artisan make:migration'
alias model='php artisan make:model'
alias control='php artisan make:controller'
alias policy='php artisan make:policy'
alias middleware='php artisan make:middleware'
alias route='php artisan route:list'
alias fix='git add . && git commit -m “fix” && git pull && git push'
alias gs='git status'
alias ni='npm install'
alias wat='npm run watch'
alias prod='npm run prod'
alias cr='composer require'
alias add='git add .'
alias pull='git pull'
alias push='git push'
alias com='git commit -m'
alias seed='php artisan db:seed'
alias dump='composer dump-autoload'
alias ci='composer install'
alias tseed='php artisan tenants:seed'
alias tpam='php artisan tenants:migrate'
alias key='php artisan key:generate'
alias request='php artisan make:request'
alias seeder='php artisan make:seeder'
alias clear='php artisan optimize:clear'
alias optimize='php artisan optimize'

