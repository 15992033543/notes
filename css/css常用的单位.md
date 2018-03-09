# css常用的单位

## px

像素

## em

相对父元素的font-size

css:

    .parent {
      font-size: 20px;
    }

    .child {
      width: 12em;
      height: 5em;
    }

html:

    <div class="parent">
      <!-- width: 20 * 12 = 240px, height: 20 * 5 = 100px -->
      <div class="child">Child</div>
    </div>

## rem

相对根元素（html）的font-size

css:

    html {
      font-size: 15px;
    }

    .parent {
      font-size: 20px;
    }

    .child {
      width: 12rem;
      height: 5rem;
      background-color: red;
    }

html:

    <div class="parent">
      <!-- width: 15 * 12 = 180px, height: 15 * 5 = 75px -->
      <div class="child">Child</div>
    </div>

## vw与wh

相对窗口的宽度与高度，比如窗口的宽度为500px，1vw = 5px，100vw = 500px

## vmin与vmax

相对于窗口宽度、高度的最大值、最小值，比如一个窗口的宽度为800px，高度为600px，1vmin = 6px，1vmax = 8px