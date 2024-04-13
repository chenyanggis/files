# CFI-Editor组件实现符合VSCode主题规范及适配主题切换

## VSCode主题token如何获取及如何实时传递

### 主题token获取

部分颜色token已在`tailwind-vscode`中声明定义，可做参考，结合VSCode实际theme规范(包括字体大小等)进行补充，可参考[VS Code theme colors](https://code.visualstudio.com/api/references/theme-color) 。

```js

module.exports=plugin(function() {}, {

 theme:{

   extend:{

     colors:{

       vscode:{

         'contrastActiveBorder':'var(--vscode-contrastActiveBorder)',

         'contrastBorder':'var(--vscode-contrastBorder)',

         'focusBorder':'var(--vscode-focusBorder)',

         'foreground':'var(--vscode-foreground)',

         'widget-shadow':'var(--vscode-widget-shadow)',

         'selection-background':'var(--vscode-selection-background)',

         'descriptionForeground':'var(--vscode-descriptionForeground)',

         'errorForeground':'var(--vscode-errorForeground)',

         'icon-foreground':'var(--vscode-icon-foreground)',

         'sash-hoverBorder':'var(--vscode-sash-hoverBorder)',

         'window-activeBorder':'var(--vscode-window-activeBorder)',

         'window-inactiveBorder':'var(--vscode-window-inactiveBorder)',

         'textBlockQuote-background':'var(--vscode-textBlockQuote-background)',

         // and more 486 tokens

          }

      }

    }

  }

})

```

### 主题token传递 @todo

Webviews are exposed inside an`--- |

|`badge` | [Badge Documentation](../src/badge/README.md) |

|`button` | [Button Documentation](../src/button/README.md) |

|`checkbox` | [Checkbox Documentation](../src/checkbox/README.md) |

|`data-grid` | [Data Grid Documentation](../src/data-grid/README.md) |

|`divider` | [Divider Documentation](../src/divider/README.md) |

|`dropdown` | [Dropdown Documentation](../src/dropdown/README.md) |

|`link` | [Link Documentation](../src/link/README.md) |

|`option` | [Option Documentation](../src/option/README.md) |

|`panels` | [Panels Documentation](../src/panels/README.md) |

|`progress-ring`| [Progress Ring Documentation](../src/progress-ring/README.md) |

|`radio` | [Radio Documentation](../src/radio/README.md) |

|`radio-group` | [Radio Group Documentation](../src/radio-group/README.md) |

|`tag` | [Tag Documentation](../src/tag/README.md) |

|`text-area` | [Text Area Documentation](../src/text-area/README.md) |

|`text-field` | [Text Field Documentation](../src/text-field/REDME.md) |

### 基于FAST封装符合VSCodetheme规范的组件

vscode-webview-ui-toolkit基础组件无法满足CFI-Editor开发需求，所以需要基于FAST-foundation封装符合VSCodeTheme风格的组件，

参考FAST库。

### 其他方式自定义封装的三方组件

以上两种组件仍无法满足需求时借助第三方组件进行封装，但封装使用前需进行讨论确认。

## FAST

### 简介
是一款基于Web Components和现代Web标准的Web解决方案集，目的是解决网站和设计、开发费的关键难点。

### 特点
- 兼容性好，不依赖任何框架，基于原生Web Component进行封装
- 扩展性强，Create new component compositions by nesting, styling foundation components, or extending existing components. The combinations are endless.
- 标准化，使用标准W3C标准标签及规范进行设计
- 主题设置系统完善

### Adaptive UI
FAST provides an innovative theming system called  `Adaptive UI` , which builds design system properties that designers use every day directly into every component.
FAST 设计的主题解决方案。

### 能力

####  @microsoft/fast-components
FAST基于Web Component封装的组件库，包含常用的基础组件Button、Input等。原理是通过foundation的封装形成html自定义标签。

#### @microsoft/fast-foundation
FAST提供的组件开发设计原则或者叫设计系统基座：Building blocks for custom design systems/component libraries。
其特性是为标准组件提供了基本组件行为和模板。
可基于此基座改造现有的组件库，以符合特定标准。

#### @microsoft/fast-element
FAST中的用于构建自定义组件库的轻量级脚本库：Lightweight library for building custom Web Components
