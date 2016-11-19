# Error wrapping functions

One day in a land far far away, I was reading some source code when I saw a call to a function named `emalloc()`. I knew of `malloc()` which is the system function for allocating memory. But what was this `emalloc()`? It turns out that what the function did was wrapped error-reporting around the `malloc()` function. This way, any calls to allocate memory using `emalloc` would make a report and exit in case of an error during its attempts to allocate the request memory. I found this to be really useful. Come to think of it, this is like exception handling in C (sort of).

```
void *emalloc(size_t size)
{
    void *p;

    p = malloc(size);
    if (!p) {
        fprintf(stderr,"Could not allocate memory\n");
        exit(EXIT_FAILURE);
    }
    return p;
}
```
