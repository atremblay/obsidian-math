
# Import library

Import one module at a time
```typescript
import {dayOfYear} from "https://deno.land/std@0.180.0/datetime/mod.ts";
```

Import all modules
```typescript
import * as dateLib from "https://deno.land/std@0.180.0/datetime/mod.ts";
```

# Install a library
One of two ways. Importing a library is done with a URL. First time we run the code, it will install it.

```typescript
import {dayOfYear} from "https://deno.land/std@0.180.0/datetime/mod.ts";
```

or directly on the command line

```bash
deno install "https://deno.land/std@0.180.0/http/mod.ts"
```


