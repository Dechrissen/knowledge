# Bash notes

## Check if file exists
```bash
FILE=/etc/file.txt
if [ -f "$FILE" ]; then
    echo "$FILE exists."
fi

```

## Check if directory exists
```bash
DIR=/etc/some_dir
if [ -d "$DIR" ]; then
    echo "$DIR exists."
fi
