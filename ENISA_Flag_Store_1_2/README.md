Here's the challenge, a .go file is given too with the link to the website :  

![1-0](1-0.png)  

There's a mention of a database : we directly plan to do a sqli

![1-1](1-1.png)  

So we see a classic signup page  

```
func getData(user User) (
    []Flag,
    error,
) {
    var flags []Flag

    req := fmt.Sprintf(`SELECT ctf, challenge, flag, points
                        FROM flags WHERE country = '%s';`, user.Country);
    rows, err := db.Query(req);
    if err != nil {
        return flags, err
    }
    defer rows.Close()

    for rows.Next() {
        var flag Flag
        err = rows.Scan(&flag.CTF, &flag.Challenge, &flag.Flag, &flag.Points)
        if err != nil {
            return flags, err
        }
        flags = append(flags, flag)
    }
    if err = rows.Err(); err != nil {
        return flags, err
    }

    return flags, nil
}
```

Returning back to the code, we have to analyse it. All the sql requests in the .go file are prepared, except one in the getData function... Does that mean that we could exploit it ? 👀  
