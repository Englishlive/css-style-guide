# BEM vs `>`

Considering below html structure at the the early stage of one product:

```html
<div class="button">
  <span class="button__text">Click me!</span>
</div>
```

```less
.button
{
  ...
  &__text
  {
    ...
  }
}
```

It looks nice and beautiful until your product manager comes in and ask you to add new color scheme and also make the button some sliding animation when it is "activated" to make it more "punchy" and "pop":

```html
<div class="button button--active">
  <div class="button__inner">
    <span class="button__text">Click me!</span>
  </div>
  <div class="button__inner--active">
    <span class="button__text--active">Activated</span>
  </div>
</div>

<div class="button--orange button--orange--active">
  <div class="button--orange__inner">
    <span class="button--orange__text">Click me!</span>
  </div>
  <div class="button--orange__inner--active">
    <span class="button--orange__text--active">Orange activated</span>
  </div>
</div>
```

```less
.button
{
  ...
  &--active {}
  &__inner
  {
    &--active {}
  }
  &__text
  {
    &--active {}
  }
  
  &--orange
  {
    &--active {}
    &__inner
    {
      &--active {}
    }
    &__text{
      &--active{}
    }
  }
}
```

Look at how messy it has became, the nesting structure of less doesn't reflect the dom structure and you will have to use your mind to trace back the tree to have a picture of what will be generated. 

Also you will have to make js add multiple `--active` classes to each element, because `xxx--active > .inner` is considered nesting and nesting is not allowed in BEM.

The third is when coding in BEM with multiple modifier, you immediately comes to the question -- which modifier goes first? `--orange` or `--active`? Which is never a question when we using multiple css classes and doing nesting.

So what if we write the same piece of code with `>` nesting.

```html
<div class="button active">
    <div class="inner">
        <span class="text">Click me!</span>
    </div>
    <div class="inner">
        <span class="text">Activated</span>
    </div>
</div>

<div class="button orange active">
    <div class="inner">
        <span class="text">Click me!</span>
    </div>
    <div class="inner">
        <span class="text">Orange activated</span>
    </div>
</div>
```

```less
.button
{
    ...
    .button-structure(@inner, @text)
    {
        > .inner
        {
            @inner();
            > .text 
            {
                @text();
            }
        }
    }

    .button-structure({},{});

    &.active
    {
        .button-structure({},{});
    }
    &.orange
    {
        .button-structure({},{});
    }

    &.orange.active
    {
        .button-structure({},{});
    }
}
```

The css class names look more readable and easy to type, the less file looks more elegance, and the javascript doesn't have to add active class all over the places.