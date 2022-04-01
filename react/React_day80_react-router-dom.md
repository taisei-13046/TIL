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











