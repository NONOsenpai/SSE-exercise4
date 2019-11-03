# SSE-exercise4
Auto git blame code for exercise 4


find ! -name temp -delete; git init
doit() { eval "$@"; shift $(($#-1)); git add .; git commit -m "$*"; }
(
doit '>file'
doit echo '>>file' line1
doit echo '>>file' line2
doit '>B'
doit sed -i "'s/line2/REFORMATTED line2/'" file '#' 'REFORMATTED line2'
doit echo '>>file' line3
) >/dev/null

set -x
git log --oneline --graph --decorate

git blame file

git config diff.REF.textconv 'awk '\''NR==2 && $1!="REFORMATTED" {$1="REFORMATTED "$1 }1'\'
mkdir .git/info
echo file diff=REF >.git/info/attributes

git blame file
