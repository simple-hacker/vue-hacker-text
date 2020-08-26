# vue-hacker-text

Vue Single File Component to animate text in different decoding styles.

![example](https://raw.githubusercontent.com/simple-hacker/vue-hacker-text/master/images/examples.gif)

# Installation (NPM)

```bash
npm install --save vue-hacker-text
```

# Basic Usage
```html
<template> 
    <vue-hacker-text text="Hacker Text"/>
<template>

<script>
    import Vue from 'vue';
    import VueHackerText from 'vue-hacker-text';

    export default Vue.extend({
        components: {
            VueHackerText
        },
    });
</script>
```

where text is the string you wish to finish decoding to.

# Props

Prop | Type | Values | Default | Description
----- | ----- | ----- | ------ | -----
text | `String` | - | - | Required
mode | `String` | `hacker`, `type`, `stars` | `hacker` | Choose decoding style
speed | `String` | `slow`, `medium`, `fast` | `medium` | The speed at which a character changes. `slow = 50ms`, `medium = 25ms`, `fast = 5ms`
probability | `Number` | `0.005 - 1.0` | `0.01` | The probability of selecting the correct character.
typeCharacter | `String` | - | `|` | Flashing character used for Type decoder


Changing the probability can fine tune the speed of which the decoding finishes.  Selecting fast speed with a high probability will result in an extremely fast decode.  Selecting slow speed with a low probability will result in a very slow decode.

The character set used for decoding is 95 characters.  So selecting a probability of less than 5% (0.005) does not make sense as it'll be more likely to choose the correct character randomly.

# Events

Event | Description
----- | ---------
`@decodeStart` | Event emitted before decoding starts
`@decodeFinish` | Event emitted once completion of decoding


# Examples

```html
<vue-hacker-text text="Hacker Text" :probability="0.35" speed="slow"/>

<vue-hacker-text text="Hacker Text" mode="stars" speed="fast"/>

<vue-hacker-text text="Hacker Text" mode="type" typeCharacter="#" :probability="0.35" speed="slow"/>

<vue-hacker-text text="Hacker Text" @decodeStart="startFunction" @decodeFinish="finishFunction"/>
```

# Styling

Wrap the component in a styled div.  The component is a simple span so it will inherit any styling from its parent div.

```html
<div class="hacker-text">
    <vue-hacker-text text="Hacker Text"/>
</div>

<style>
    .hacker-text {
        font-weight: 600;
        font-size: 2em;
        color: blue;
    }
</style>

```

# License

[The MIT License](http://opensource.org/licenses/MIT)