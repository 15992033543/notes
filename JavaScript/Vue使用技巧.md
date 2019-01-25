# Vue使用技巧

## 使用v-bind与this.$attrs、v-on与this.$listeners优雅的封装二次组件

    <template>
      <el-pagination
        layout="total, sizes, prev, pager, next, jumper"
        :page-sizes="[ 10, 20, 50, 100 ]"
        v-bind="$attrs"
        v-on="$listeners"/>
    </template>

## props接收多个类型

    props: {
      type: [ String, Number ],
      ...
    }

## 使用this.$slots判断是否定义了某个slot

    computed: {
      hasHeader () {
        return this.$slots.header
      }
    }