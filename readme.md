# DKF-Spinner

This is an advanced loading component for Vue 2+. It allows you to display a
loading spinner that is always visible for a minimal amount of time.

Demo: https://dkfbasel.github.io/dkf-spinner/

The idea behind this is, that the user will always get a feedback that data is
loaded from the server. The loading indicator will stay visible, if the data
fetching is faster than the minimal time set in the configuration.
If the data fetching takes longer, the indicator will be hidden immediately
after data fetching (respectively when you update the respective property).

Example usage:

- set the variable `loading` to `true` before you start loading the data to start
the loading indicator
- set the variable `loading` to `false` as soon as the data arrives and is ready
to be rendered
- wait for the `@loaded` event to be fired to toggle the visibility of your data.

```
<dkf-spinner :loading="loading"	@loaded="toggleDataVisibility"></dkf-spinner>
```

A complete example can be found on the demo page.

Please let us know, if you require any assistance or have ideas for improvements.
