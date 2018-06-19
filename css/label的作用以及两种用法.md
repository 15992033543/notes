# label的作用以及两种用法

label与表单元素绑定使用，用于说明这个表单元素是干什么的，而且在点击label时可以对表单元素进行聚焦。

用法一：使用for与input的id进行关联。

    <label for="username">用户名：</label>
    <input id="username" type="text" />

用法二：将input放至lable中。

    <label>
      用户名：
      <input type="text" />
    </label>