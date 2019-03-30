```bash
echo "Logged in as $USER at $(hostname)"

################################################################################
#   COMPANY
################################################################################

################################################################################
#   Bash CMDS
################################################################################

alias ls="ls -Gh"
# Cycles wifi on and off to reset
fuckingwifi () {
 networksetup -setairportpower en0 off
 networksetup -setairportpower en0 on
 sleep 1
 echo "Fucking WiFi..."
 sleep 1
 echo "I swear to god, if Steve Jobs wasn't dead..."
 sleep 1
 echo "I'd punch him in his stupid turtleneck..."
}

################################################################################
#   Git
################################################################################

# Load git completions
git_completion_script=/usr/local/etc/bash_completion.d/git-completion.bash
test -s $git_completion_script && source $git_completion_script

git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.last 'log -1 HEAD'

# Branch clean or dirty
git_prompt () {
 # Git directory?
 if ! git rev-parse --git-dir > /dev/null 2>&1; then
 return 0
 fi
 # Working branch
 git_branch=$(git branch 2>/dev/null| sed -n '/^\*/s/^\* //p')
 # Clean or dirty branch
 if git diff --quiet 2>/dev/null >&2; then
 git_color="${c_git_clean}"
 else
 git_color=${c_git_dirty}
 fi
 echo " [$git_color$git_branch${c_reset}]"
}

################################################################################
#   rbEnv / Ruby / Rails
################################################################################

eval "$(rbenv init -)"
alias be="bundle exec"

################################################################################
#   Styling
################################################################################

# A more colorful prompt
c_reset='\[\e[0m\]' # Reset default color
c_path='\[\e[0;31m\]' # Red
c_git_clean='\[\e[0;32m\]' # Green
c_git_dirty='\[\e[0;31m\]' # Red
# PS1 is the variable for the prompt you see everytime you hit enter
if [ $OSTYPE == 'darwin15' ] && ! [ $ITERM_SESSION_ID ]
then
 PROMPT_COMMAND=$PROMPT_COMMAND'; PS1="${c_path}\W${c_reset}$(git_prompt) :> "'
else
 PROMPT_COMMAND=$PROMPT_COMMAND' PS1="${c_path}\W${c_reset}$(git_prompt) :> "'
fi

# Colors ls should use for folders, files, symlinks etc, see `man ls` and
# search for LSCOLORS
export LSCOLORS=ExGxFxdxCxDxDxaccxaeex
# Force grep to always use the color option and show line numbers
export GREP_OPTIONS='--color=always'
```
