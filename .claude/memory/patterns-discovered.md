# Patterns Discovered

Code patterns and solutions that work well in this project. When you discover a pattern that solves a recurring problem, document it here.

**Format:** Each pattern has: name, context, problem, solution, code example, related files, trade-offs.

---

## Pattern: Async Error Handling in Express Middleware

**Context:** When: Writing Express middleware that handles async operations  
Where: API route handlers, middleware chains  
Why: Express doesn't automatically catch promise rejections in async middleware

**Problem:**
- Promise rejections in async middleware are silently ignored
- Errors don't propagate to Express error handler
- Users see hanging requests instead of error responses
- Debugging is difficult because errors disappear

**Solution:**
Wrap async middleware in a simple try-catch wrapper function. This catches both synchronous errors and promise rejections.

**Code Example:**
```javascript
// Wrapper function for async middleware
const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

// Usage in middleware
app.get('/protected', asyncHandler(async (req, res) => {
  // This will be caught and passed to error handler
  const user = await validateToken(req.headers.authorization);
  res.json({ user });
}));

// Or with try-catch for more control
app.post('/login', asyncHandler(async (req, res) => {
  try {
    const token = await generateToken(req.body);
    res.json({ token });
  } catch (error) {
    res.status(401).json({ error: 'Authentication failed' });
  }
}));
```

**Related Files:**
- src/middleware/auth.js (line 15) - JWT validation middleware
- src/middleware/errorHandler.js (line 5) - Central error handler
- tests/middleware/auth.test.js (line 30) - Error handling tests

**Trade-offs:**
- ✅ Simple and easy to understand
- ✅ Works with Express error handlers
- ✅ Can be extracted to a utility function
- ❌ Requires wrapping every async middleware
- ❌ Some developers prefer dedicated async middleware libraries

**When to Use:**
- ✅ DO: Use for auth, validation, database middleware
- ❌ DON'T: Don't overthink; this pattern is lightweight and reliable

**When NOT to Use:**
- If you prefer a library like express-async-errors
- If your team has a different preference (document it)

---

## Pattern: Service Initialization (Empty Array vs Null)

**Context:** When: Initializing dependencies in service classes  
Where: Service constructors, initialization methods  
Why: Different behaviors for "not set" vs "set to empty"

**Problem:**
- Is `[]` initialized but empty, or uninitialized?
- Is `null` means not provided, or intentionally empty?
- Leads to bugs when checking truthiness: `if (items)` fails for empty arrays

**Solution:**
- Use `null` to indicate "not provided/not initialized"
- Use `[]` or `{}` to indicate "initialized but empty"
- Always check with explicit comparisons: `if (items !== null)` or `if (items?.length > 0)`

**Code Example:**
```javascript
// Service with dependency initialization
class UserService {
  constructor(database = null, cache = null) {
    // null = not provided, [] = initialized but empty
    this.database = database;
    this.cache = cache;
  }

  async getUser(id) {
    // Check: is cache initialized?
    if (this.cache !== null) {
      const cached = await this.cache.get(`user:${id}`);
      if (cached) return cached;
    }

    // Use database (required)
    const user = await this.database.query('SELECT * FROM users WHERE id = ?', [id]);
    
    // Update cache if available
    if (this.cache !== null) {
      await this.cache.set(`user:${id}`, user);
    }

    return user;
  }
}

// Usage
const service = new UserService(database); // cache is null (not provided)
const serviceWithCache = new UserService(database, cacheClient); // cache is initialized
```

**Related Files:**
- src/services/userService.js (line 5) - User service with optional cache
- src/services/databaseService.js (line 12) - Database dependency initialization
- tests/services/userService.test.js (line 45) - Test cases for both scenarios

**Trade-offs:**
- ✅ Clear intent: null = optional, [] = initialized
- ✅ Prevents truthiness bugs
- ✅ Easy to test both with/without optional dependencies
- ❌ Requires explicit null checks (slightly verbose)
- ❌ Different from JavaScript's typical "empty array is falsy" convention

**When to Use:**
- ✅ DO: Use for optional dependencies (cache, logging, analytics)
- ✅ DO: Use for arrays/objects that might be empty but are intentionally so
- ❌ DON'T: Don't use for required dependencies (those should error if missing)

**When NOT to Use:**
- If a dependency is required (use early throw instead)
- If you prefer optional chaining: `this.cache?.get()` (also valid)

---

## Template for New Patterns

Use this template when documenting a new pattern:

```
## Pattern: [Pattern Name]

**Context:** When/Where/Why this pattern applies

**Problem:** What problem does this pattern solve?

**Solution:** How does it solve the problem?

**Code Example:**
[Working code example]

**Related Files:**
- src/file.js (line X) - Brief description
- tests/file.test.js (line Y) - Brief description

**Trade-offs:**
- ✅ Pros
- ❌ Cons

**When to Use:**
- ✅ DO: Good for...
- ❌ DON'T: Not good for...

**When NOT to Use:**
Alternatives or when this pattern doesn't apply
```

---

**Remember:** Document patterns after you've used them successfully 2-3 times. If you keep solving the same problem the same way, it's probably a pattern worth documenting.
