### Chip

> #### RawChip

Material风格标签控件，RawChip是其他标签控件的基类，通常不会直接创建该控件，而是使用以下控件：

* Chip
* InputChip
* ChoiceChip
* FilterChip
* ActionChip

但如果想自定义标签控件时可以使用此控件

RawChip可以通过设置onSelected被选中，设置onDeleted被删除，还可以设置onPressed而像一个按钮，它有一个label属性，一个前置图标(avatar) 和后置图标(deleteIcon)

```
Key key,
this.avatar,
@required this.label,
this.labelStyle,
this.padding,
this.visualDensity,
this.labelPadding,
Widget deleteIcon,
this.onDeleted,
this.deleteIconColor,
this.deleteButtonTooltipMessage,
this.onPressed,
this.onSelected,
this.pressElevation,
this.tapEnabled = true,
this.selected = false,
this.isEnabled = true,
this.disabledColor,
this.selectedColor,
this.tooltip,
this.shape,
this.clipBehavior = Clip.none,
this.focusNode,
this.autofocus = false,
this.backgroundColor,
this.materialTapTargetSize,
this.elevation,
this.shadowColor,
this.selectedShadowColor,
this.showCheckmark = true,
this.checkmarkColor,
this.avatarBorder = const CircleBorder(),
```