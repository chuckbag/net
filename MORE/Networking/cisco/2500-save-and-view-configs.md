# Save and View configs

Remove paging so that all of the config is shown at once, and you don't need to enter <return> a million times. 
```
config paging disable
```

Then save your configs: 
```
save config
```

and then show the running config: 
```
show run-config commands
```

The configs that are spewed out are a bit messy.  In [VIM](https://www.vim.org/), you can clean this up with the following two commands: 

remove all preceding whitespaces: 
```
:%s/$ //
```

remove all empty lines
```
:g/^$/d
```

