# Controlled and Uncontrolled Components

One of the strengths of React is its extensibility model, enabled by a common practice of pushing component state
as high up the component hierarchy as possible. For simple components (like an input) this
is usually a matter of tying the input `value` prop to a parent state property via its `onChange` handler.
here is an extremely common pattern:

```js
render() {
  return (
    <input type='text'
      value={this.state.value}
      onChange={ e => this.setState({ value: e.target.value })}
    />
  )
}
```

This pattern moves the responsibility of managing the `value` prop from the input Component to its parent.
This is called a __controlled__ component because the parent is in complete _control_ of setting the `value` prop,
in fact the input couldn't change its value even if it wanted to, it will _always_ render the `value` prop it is given.

Using controlled components is great for flexibility, and helps keep the data flow going in one direction,
but it can become tedious to wire up the components every time, even if you don't need the benefits of controlling it.
To mitigate this concern React introduces the concept of an __uncontrolled__ Component.

```js
render() {
  return (
    <input type='text'
      defaultValue={'hello'}
    />
  )
}
```

This input, doesn't provide a `value` prop, instead it uses the `defaultValue` prop to set an _initial_
value for the input. After that initial setting, the input takes over and manages the value itself requiring
no more input from the parent. This is much simpler to set up but, is not conducive to setting the input value
from _outside_ the input. In this case however, that isn't needed so we can leave it uncontrolled.

We can extend this pattern to more than just `value` props, lots of things can be controlled or uncontrolled.
In `react-formal` for instance the `Form` component lets you control the `errors` prop for setting
the current errors in the form.

```js
// controlled
<Form
  errors={this.state.errors}
  onError={errors => this.setState({ errors })}
>
  {/* fields omitted */}
</Form>

// Uncontrolled
<Form>
  {/* fields omitted */}
</Form>

// and Uncontrolled with an initial setting
<Form defaultErrors={this.state.errors}>
  {/* fields omitted */}
</Form>
```

So when a prop is said to be "controllable" it means that you have the option to let the component handle it, or if you need finer grained control over how that prop updates you can jump in and handle it yourself.
