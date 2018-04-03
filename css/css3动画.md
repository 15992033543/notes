# css3动画

css3动画分为transition与animation

## transition

transition用于制作过渡效果。它可以为元素指定一个css属性，当元素的这个css属性发生变化时，就会产生过渡效果。

属性：

|             值            |         作用         |
| --------------------------- | ------------------- |
| transition-property | 规定设置过渡效果的 CSS 属性的名称 |
| transition-duration | 完成过渡效果需要多少秒或毫秒 |
| transition-timing-function | 规定速度效果的速度曲线 |
| transition-delay | 延迟动画的开始时间 |

单行简写：transition: property duration timing-function delay

## animation

animation相当于transition的增强版，能够制作出更灵活的过渡效果。

属性：

|        值        |      作用      |
| --------------- | -------------- |
| animation-name | 要绑定的keyframe名称 |
| animation-duration | 完成过渡效果需要多少秒或毫秒 |
| animation-timing-function | 规定速度效果的速度曲线 |
| animation-delay | 延迟动画的开始时间 |
| animation-iteration-count | 动画的播放次数（infinite为无限次） |
| animation-direction | 是否轮流反向播放动画（alternate为轮流反向播放） |
| animation-fill-mode | 指定动画执行完毕后元素最终的状态（forwards为停留在最后一帧） |
| animation-play-state | 指定动画运行还是暂停（paused为暂停，running为运行） |

单行简写：animation: name duration timing-function delay iteration-count direction fill-mode play-state

@keyframes：

keyframes指定过渡开始的状态以及结束的状态

    @keyframes my-frames {
      from { transform: translateX(0); }
      to { transform: translateX(300px); }
    }

也可以指定过渡执行时每个阶段的状态（from和to其实代表的就是0%和100%）

    @keyframes my-frames {
      0% { transform: translateX(0); }
      60% { transform: translateX(300px); }
      100% { transform: translateX(220px); }
    }

## timing-function的预设值

|             值            |         作用         |
| --------------------------- | ------------------- |
| linear | 规定以相同速度开始至结束的过渡效果（等于 cubic-bezier(0,0,1,1)） |
| ease | 规定慢速开始，然后变快，然后慢速结束的过渡效果（cubic-bezier(0.25,0.1,0.25,1)） |
| ease-in | 规定以慢速开始的过渡效果（等于 cubic-bezier(0.42,0,1,1)） |
| ease-out | 规定以慢速结束的过渡效果（等于 cubic-bezier(0,0,0.58,1)） |
| ease-in-out | 规定以慢速开始和结束的过渡效果（等于 cubic-bezier(0.42,0,0.58,1)） |
| cubic-bezier(n,n,n,n) | 在 cubic-bezier 函数中定义自己的值。可能的值是 0 至 1 之间的数值 |