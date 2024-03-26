```bash
cat urls.txt | kxss | sed 's/=.*/=/' | sed 's/URL://' | dalfox pipe
```

