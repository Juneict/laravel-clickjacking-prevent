# laravel-clickjacking-prevent
A Documentation About How to Prevent Click Jacking In Laravel and How to set X-Frame-Options headers in Laravel

Clickjacking is a security vulnerability where an attacker can trick a user into clicking on something different from what the user perceives, potentially leading to unintended actions. Laravel, like any web application framework, provides ways to mitigate clickjacking. Here are some steps you can take to prevent clickjacking in a Laravel application:

# 1 . HTTP Header: X-Frame-Options:
Set the X-Frame-Options HTTP header to control whether a browser should be allowed to render a page in a frame or iframe. You can set this header in your Laravel application by adding the following code to your middleware or directly in your routes file:

```
// In a middleware
public function handle($request, Closure $next)
{
    $response = $next($request);
    $response->header('X-Frame-Options', 'DENY');
    return $response;
}
```
If you want to allow your site to be framed by a specific domain, you can use:
```
$response->header('X-Frame-Options', 'ALLOW-FROM https://example.com');
```

# 2 . Content Security Policy (CSP):
Implementing a Content Security Policy is another layer of defense. CSP allows you to define which sources are allowed to load resources on your page. To set up CSP, you can use Laravel's ContentSecurityPolicy middleware or add headers directly in your routes file:
```
// In a middleware
public function handle($request, Closure $next)
{
    $response = $next($request);
    $response->header('Content-Security-Policy', 'frame-ancestors \'none\'');
    return $response;
}
```
This will prevent the page from being embedded in an iframe.

#3 . Frame Busting JavaScript:
Implement JavaScript on your pages to prevent them from being framed. Add the following script to your HTML:
```
<script type="text/javascript">
    if (top != self) {
        top.location.href = self.location.href;
    }
</script>
```
This script checks if the current window is the top-level window and, if not, redirects the user to the top-level window.

By implementing these measures, you can enhance the security of your Laravel application and reduce the risk of clickjacking attacks. Always stay informed about the latest security practices and consider consulting Laravel's documentation for any updates or additional security features.
