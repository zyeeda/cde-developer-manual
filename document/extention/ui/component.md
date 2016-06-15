## Components
控件库， View 更小的单元，它只能依靠 View 而存在，它有很多种类型，所有类型组件都具以下属性：

- id       控件id
- name     控件名称
- value    值
- label    显示名称
- hideLabel 是否隐藏label， 默认false
- readOnly 是否只读， 默认false
- disabled 是否禁用， 默认false
- required 是否必填， 默认false
- visible  是否显示， 默认true
- type     类型， 默认text
- colspan  跨列，默认是1
- rowspan  跨行，默认是1
- statusChanger 状态开关，change事件会触发formStatusChanged事件，默认 false

以下是组件的用法：
```js
define({
    type: 'form-view',
    formName: 'test',
    labelOnTop: false,
    avoidLoadingHandlers: true,

    fieldGroups: {
        group1: [
            {name: 'field1', type: 'mask', pattern: '9999-99-99', label: '日期'},
            {name: 'field1', type: 'mask', pattern: '199-9999-9999', label: '手机号码'},
            {name: 'field1', type: 'mask', pattern: 'aaaa-aaaa-aaaa-aaaa', label: '序列号'},
            'field1',
            {name: 'field5', type: 'datepicker', label: 'Date Picker'}
        ],
        group2: [
            {name: 'field6', type: 'dropdown', label: 'Drop down', source: [{id: 'a', text: 'A'}, {id: 'b', text: 'B'}]},
            {name: 'field7', type: 'hidden'},
            {name: 'field12', type: 'grid-picker', source: 'test/trees', label: 'Grid Picker'},
            {name: 'field13', type: 'tree-picker', source: 'test/trees', label: 'Tree Picker'},
            {name: 'field14', type: 'file-picker', url: 'invoke/simple/foo/first', label: 'File Picker', acceptFileTypes: /(\.|\/)(gif|jpe?g|png)$/i},
            {name: 'field8', type: 'textarea', colspan: 2, rowspan: 5}
        ],
        group4: [
            {name: 'field10', label: 'Pined'}
        ],
        group5: ['field11', 'field9']
    },
    form: {
        groups: [
            {name: 'group1', label: 'Group 1'},
            {name: 'group2', label: 'Group 2', columns: 2},
            {name: 'group4', label: 'Group 4'},
            {name: 'group5', label: 'Group 5'}
        ]
    }
});

```

### 文本组件 text
- type='text'

### 规则文本组件 mask
- type='mask'
- pattern 规则  199-9999-9999|9999-99-99

### 数字范围组件 number-range
- type='number-range'

### 隐藏组件 hidden 
- type='hidden'

### 大文本组件 textarea
- type='textarea'
- rows textarea 高度，默认是3

### 下拉列表 dropdown
- type='dropdown'
- textKey 显示字段，默认是name
- url ajax数据源
- source 静态数据源
- defaultValue 默认选中值
- change 事件

### 日期组件 datepicker
- type='datepicker'

### 日期范围组件 date-range
- type='date-range'

### 文件组件 file-picker
- type='file-picker'
- acceptFileTypes 允许选择的文件类型
- multiple 是否支持多附件上传
- preview 预览位置 top|right|bottom|left
- url 上传地址

### 树选择组件 tree-picker
- type='tree-picker'
- root 根节点
- source ajax数据源
- data 静态数据
- [ztree](http://www.ztree.me/v3/api.php) API 

### grid
- paginate 是否分页
- checkBoxColumn 是否显示选中控件列
- numberColumn  是否显示num列
- defaultOrder 默认列表排序列 'name-asc, birthday-desc'
- data 静态数据源
- fixedHeader 是否锁列头
- [datatables](http://datatables.net/reference/option/) API 

### 列表选择组件 grid-picker
- type='grid-picker'
- title 选择框标题
- source ajax数据源
- multiple 多选开关
- textKey 确认选中的显示字段
- crossPage 支持跨页选中
- allowAdd 允许增加

### 内嵌选择组件 inline-gird 继承列表选择组件
- allowPick  允许选择
- allowAdd   允许添加
- allowEdit  允许编辑
- disableShow 禁用查看
- [datatables](http://datatables.net/reference/option/) API 