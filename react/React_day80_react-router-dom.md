## やったこと
react-router-domを読む


### useLocation

The useLocation hook returns the location object that represents the current URL. You can think about it like a useState that returns a new location whenever the URL changes.  

```tsx
function usePageViews() {
  let location = useLocation();
  React.useEffect(() => {
    ga.send(["pageview", location.pathname]);
  }, [location]);
}
```

### useParams

useParams returns an object of key/value pairs of URL parameters. Use it to access match.params of the current `<Route>`. 

### useRouteMatch

The useRouteMatch hook attempts to match the current URL in the same way that a `<Route>` would. It’s mostly useful for getting access to the match data without actually rendering a `<Route>`.

### `<BrowserRouter>`

```tsx
<BrowserRouter
  basename={optionalString}
  forceRefresh={optionalBool}
  getUserConfirmation={optionalFunc}
  keyLength={optionalNumber}
>
  <App />
</BrowserRouter>
```

#### basename: string
The base URL for all locations. If your app is served from a sub-directory on your server, you’ll want to set this to the sub-directory. A properly formatted basename should have a leading slash, but no trailing slash.  

```tsx
<BrowserRouter basename="/calendar">
    <Link to="/today"/> // renders <a href="/calendar/today">
    <Link to="/tomorrow"/> // renders <a href="/calendar/tomorrow">
    ...
</BrowserRouter>
```

#### forceRefresh: bool
If true the router will use full page refreshes on page navigation. You may want to use this to imitate the way a traditional server-rendered app would work with full page refreshes between page navigation.  

#### keyLength: number
The length of location.key. Defaults to 6.

### `<Link>`  

#### to: string
A string representation of the Link location, created by concatenating the location’s pathname, search, and hash properties.

```tsx
<Link to="/courses?sort=name" />
```

#### to: object
An object that can have any of the following properties:
- **pathname**: A string representing the path to link to.
- **search**: A string representation of query parameters.
- **hash**: A hash to put in the URL, e.g. #a-hash.
- **state**: State to persist to the location.














