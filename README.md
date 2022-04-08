<div align="center">
<img alt="Prettier"
  src="https://raw.githubusercontent.com/prettier/prettier-logo/master/images/prettier-icon-light.png">
<img alt="PHP" height="180" hspace="25" vspace="15"
  src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/9a/Laravel.svg/500px-Laravel.svg.png">
</div>

<h2 align="center">Prettier Blade Plugin</h2>
<p align="center">
  <a href="https://github.com/shufo/prettier-plugin-blade/actions">
    <img alt="GitHub Workflow Status" src="https://img.shields.io/github/workflow/status/shufo/prettier-plugin-blade/Node CI?style=flat-square">
  </a>
  <a href="https://www.npmjs.com/package/@prettier/plugin-php">
    <img alt="npm version" src="https://img.shields.io/npm/v/@shufo/prettier-plugin-blade.svg?style=flat-square">
  </a>
</p>

Format your blade template using Prettier

## Features

- Automatically Indents markup inside directives
- Automatically add spacing to blade templating markers
- PHP 8 syntax support (null safe operator, named arguments)
- Compliant to PSR-2 coding standard (PHP code inside directives)
- Automatically sort Tailwind CSS classes with respect of `tailwind.config.js`

## Installation

```bash
$ npm install --save-dev @shufo/prettier-plugin-blade prettier
# yarn
$ yarn add -D @shufo/prettier-plugin-blade prettier
```

## Usage (CLI)

```bash
$ ./node_modules/.bin/prettier --write resources/**/*.blade.php
```

Update your `.prettierrc.json` file to include the new plugin:

```json
{
    "plugins": ["@shufo/prettier-plugin-blade"]
}
```

https://user-images.githubusercontent.com/1641039/151354641-6305805e-8e0c-4226-8331-64195f85160e.mp4

## Example

### Input

```blade
@extends('frontend.layouts.app')
@section('title') foo
@endsection
@section('content')
<section id="content">
<div class="container mod-users-pd-h">
    <div class="pf-user-header">
    <div></div>
    <p>@lang('users.index')</p>
    </div>
        <div class="pf-users-branch">
            <ul class="pf-users-branch__list">
                @foreach($users as $user)
        <li>
            <img src="{{ asset('img/frontend/icon/branch-arrow.svg') }}" alt="branch_arrow">
            {{ link_to_route("frontend.users.user.show",$users["name"],$users['_id']) }}
        </li>
        @endforeach
      </ul>
      <div class="pf-users-branch__btn">
      @can('create', App\Models\User::class)
            {!! link_to_route("frontend.users.user.create",__('users.create'),[1,2,3],['class' => 'btn']) !!}
            @endcan
        </div>
  </div>
    </div>
</section>
@endsection
@section('footer')
@stop
```

### Output

```blade
@extends('frontend.layouts.app')
@section('title') foo
@endsection
@section('content')
    <section id="content">
        <div class="container mod-users-pd-h">
            <div class="pf-user-header">
                <div></div>
                <p>@lang('users.index')</p>
            </div>
            <div class="pf-users-branch">
                <ul class="pf-users-branch__list">
                    @foreach ($users as $user)
                        <li>
                            <img src="{{ asset('img/frontend/icon/branch-arrow.svg') }}" alt="branch_arrow">
                            {{ link_to_route('frontend.users.user.show', $users['name'], $users['_id']) }}
                        </li>
                    @endforeach
                </ul>
                <div class="pf-users-branch__btn">
                    @can('create', App\Models\User::class)
                        {!! link_to_route('frontend.users.user.create', __('users.create'), [1, 2, 3], ['class' => 'btn']) !!}
                    @endcan
                </div>
            </div>
        </div>
    </section>
@endsection
@section('footer')
@stop
```

## Options

You can use these options for prettier blade plugin in prettier CLI.

|                          key |                                                                                                                                                   description |
| ---------------------------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|                `--tab-width` |                                                                                                          Number of spaces per indentation level. default: `4` |
|              `--print-width` |                                                                                                  The line length where Prettier will try wrap. default: `120` |
|          `--wrap-attributes` | The way to wrap attributes. [`auto`\|`force`\|`force-aligned`\|`force-expand-multiline`\|`aligned-multiple`\|`preserve`\|`preserve-aligned`]. default: `auto` |
|        `--end-with-new-line` |                                                                                                                      End output with newline. default: `true` |
| `--sort-tailwindcss-classes` |                                                       Sort Tailwind CSS classes. It will lookup and respects `tailwind.config.js` if exists. default: `false` |

### `.prettierrc` example

```json
{
  "printWidth": 120,
  "tabWidth": 4,
  "wrapAttributes": "auto",
  "sortTailwindcssClasses": true
}
```

## Editor Integration

Below editors are confirmed working with this plugin.

### VSCode

You can use [Prettier extension for VSCode](https://github.com/prettier/prettier-vscode) to format blade within VSCode. You must install this plugin as local dependencies. see https://github.com/prettier/prettier-vscode#prettier-resolution

If you want to use formatter without Prettier, please consider to using [vscode-blade-formatter](https://github.com/shufo/vscode-blade-formatter)

### Vim

You can use [coc-prettier](https://github.com/neoclide/coc-prettier) plugin on [coc.nvim](https://github.com/neoclide/coc.nvim)

If you want to use formater without Prettier, please consider to using [coc-blade](https://github.com/yaegassy/coc-blade)

### JetBrains WebStorm, PHPStorm, PyCharm...

You can use [Prettier Plugin](https://plugins.jetbrains.com/plugin/10456-prettier) for JetBrains IDE.

Add extension setting `blade.php` to `File | Settings | Languages & Frameworks | JavaScript | Prettier | Run for files`:

e.g.

`{**/*,*}.{js,ts,jsx,tsx,blade.php}`

and turn on checkbox `On 'Reformat Code' action`

## Limitation

This plugin is based on [blade-formatter](https://github.com/shufo/blade-formatter) that does not generate ASTs with lexer, so it might be break indentation on complex blade.

Like:

- The mix of open/closed HTML tag and directives

❌ Example of **unexpected** code

```blade
@if ($user)
  <div>
@else
  </div>
@endif
```

⭕ Example of **expected** code

```blade
@if ($user)
  <div>foo</div>
@else
  <div>bar</div>
@endif
```

Please make blade template as simple as possible for better formatting.

## API

You can format blade text programmatically using prettier

```js
const prettier = require("prettier");

const input = `
<div>
  @if ($user)
  {{ $foo }}
  @else
  {{ $bar }}
  @endif
</div>
`;

const res = prettier.format(input, { parser: "blade" });
console.log(res);
// =>
//<div>
//    @if ($user)
//        {{ $foo }}
//    @else
//        {{ $bar }}
//    @endif
//</div>
```

## Development

```bash
$ yarn install
$ yarn run watch # watch changes
```

## Testing

```bash
$ yarn install
$ yarn run test
```

## Contributing

1.  Fork it
2.  Create your feature branch (`git checkout -b my-new-feature`)
3.  Commit your changes (`git commit -am 'Add some feature'`)
4.  Push to the branch (`git push origin my-new-feature`)
5.  Create new Pull Request

## Contributors

<!-- readme: collaborators,contributors -start -->
<table>
<tr>
    <td align="center">
        <a href="https://github.com/shufo">
            <img src="https://avatars.githubusercontent.com/u/1641039?v=4" width="100;" alt="shufo"/>
            <br />
            <sub><b>Shuhei Hayashibara</b></sub>
        </a>
    </td>
    <td align="center">
        <a href="https://github.com/ianjamieson">
            <img src="https://avatars.githubusercontent.com/u/2786904?v=4" width="100;" alt="ianjamieson"/>
            <br />
            <sub><b>Ian Jamieson</b></sub>
        </a>
    </td></tr>
</table>
<!-- readme: collaborators,contributors -end -->

## LICENSE

MIT
