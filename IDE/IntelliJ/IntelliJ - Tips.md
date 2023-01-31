## Change Git signed-off userName and email

```
# global config method
C:\Users\<USER_DIR>\.gitconfig 
[user]
    name = <USERNAME>
    email = <EMAIL>
```

```
# project config method
1. Go to the base directory of your project.
2. You will find a hidden directory called .git. Enter into it.
3. There you will see a file called config. Add the following code.
[user]
      name = username
      email = username@domain.example
```
