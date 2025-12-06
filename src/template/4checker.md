```
alias t='for f in *.in; do b=${f%.in}; diff <(./a.out < $f) $b.ans >/dev/null && echo $b:OK || echo $b:WA; done'
source ~/.bashrc
```