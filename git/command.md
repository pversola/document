# Create a new repository

```
echo "# test2" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/pversola/test2.git
git push -u origin main
```

# Push an existing repository

```
git remote add origin https://github.com/pversola/test2.git
git branch -M main
git push -u origin main
```
