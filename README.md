# Cloudflare-Workers-Tarpit
Cloudflare Workers script that can Tarpit requests, useful for black-holing bad bots.

```
export default {
    async fetch(request, env) {
        try {
            let {pathname} = new URL(request.url)
            if (pathname.startsWith("/status")) {
                const httpStatusCode = Number(pathname.split("/")[2]) || 404
                const delay = Number(pathname.split("/")[3]) || 0
                await new Promise(resolve => setTimeout(resolve, delay * 1000))
                return Number.isInteger(httpStatusCode) ? fetch("https://example.com/" + httpStatusCode) : new Response("That's not a valid HTTP status code.")
            }
            return fetch("https://welcome.developers.workers.dev")
        } catch (e) {
            return new Response(e.stack, {status: 500})
        }
    }
}
```
