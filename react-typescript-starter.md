---
title: TypeScript
category: JavaScript libraries
---

TypeScript is just like ES2015 with type-checking. All ES2015 (classes, etc) should work.

## Basic types

```ts
any
void

boolean
number
string

null
undefined

string[]          /* or Array<string> */
[string, number]  /* tuple */

string | null | undefined   /* union */

never  /* unreachable */
```

```ts
enum Color {Red, Green, Blue = 4}
let c: Color = Color.Green
```

## Declarations

```ts
let isDone: boolean
let isDone: boolean = false
```

```ts
function add (a: number, b: number): number {
  return a + b
}

// Return type is optional
function add (a: number, b: number) { ... }
```

## Type assertions

#### Variables
```ts
let len: number = (input as string).length
let len: number = (<string> input).length  /* not allowed in JSX */
```

#### Functions
```ts
function object(this: {a: number, b: number}, a: number, b: number) {
  this.a = a;
  this.b = b;
  return this;
}

// this is used only for type declaration
let a = object(1,2);
// a has type {a: number, b: number}
```


## Interfaces

### Inline

```ts
function printLabel (options: { label: string }) {
  console.log(options.label)
}

// Note the semicolon
function getUser (): { name: string; age?: number } {
}
```

### Explicit

```ts
interface LabelOptions {
  label: string
}

function printLabel(options: LabelOptions) { ... }
```

### Optional properties

```ts
interface User {
  name: string,
  age?: number
}
```

### Read only

```ts
interface User {
  readonly name: string
}
```

### Dynamic keys

```ts
{
  [key: string]: Object[]
}
```

## Type aliases

```ts
type Name = string | string[]
```

## Function types

```ts
interface User { ... }

function getUser(callback: (user: User) => any) { callback({...}) }

getUser(function (user: User) { ... })
```

## Classes

```ts
class Point {
  x: number
  y: number
  static instances = 0
  constructor(x: number, y: number) {
    this.x = x
    this.y = y
  }
}
```

#### Inheritance

```ts
class Point {...}

class Point3D extends Point {...}

interface Colored {...}

class Pixel extends Point implements Colored {...}
```

#### Short fields initialisation

```ts
class Point {
  static instances = 0;
  constructor(
    public x: number,
    public y: number,
  ){}
}
```

#### Fields which do not require initialisation
```ts
class Point {
  public someUselessValue!: number;
  ...
}
```

## Generics

```ts
class Greeter<T> {
  greeting: T
  constructor(message: T) {
    this.greeting = message
  }
}

let greeter = new Greeter<string>('Hello, world')
```

## Modules

```ts
export interface User { ... }
```

## Type extraction

```ts
interface Building {
  room: {
    door: string,
    walls: string[],
  };
}

type Walls = Building['room']['walls']; // string[]
```

# framework

## cssinjs global css & var
```ts
import { createUseStyles } from 'react-jss'

const useStyles = createUseStyles({
    'root': ({ theme }: IThemedStyleProps) => ({
        '& path': {
            stroke: theme.colors.contentPrimary,
        },
        ...Object.entries(theme.colors).reduce((p: Record<string, string>, [k, v]) => {
            return {
                ...p,
                [`--color-${k}`]: v,
            }
        }, {} as Record<string, string>),
    }),
    '@global': {
        '.react-lazylog': {
            background: 'var(--color-backgroundPrimary)',
        },
        '.react-lazylog-searchbar': {
            background: 'var(--color-backgroundPrimary)',
        },
        '.react-lazylog-searchbar-input': {
            background: 'var(--color-backgroundPrimary)',
        },
    },
})
```

## baseui/useStyletron

```ts
import * as React from 'react';
import {useStyletron} from 'baseui';
export default function AccentText({children}) {
  const [css, theme] = useStyletron();
  return (
    <span
      className={css({
        color: theme.colors.accent,
      })}
    >
      {children}
    </span>
  );
}
```

```ts
import {createDarkTheme} from 'baseui';
const primitives = {
  accent: '#F127E4', // hot pink
  accent50: '#FDEDFC',
};
const overrides = {
  colors: {
    buttonSecondaryFill: primitives.accent100,
  },
  typography: {
    DisplayLarge: {
      fontFamily: '',
    },
  }
};
const darkTheme = createDarkTheme(primitives, overrides);
```

```ts

export default {
  light: createLightTheme(primitives, overrides),
  dark: createDarkTheme(primitives, overrides),
  deep: createTheme(primitives, overrides),
  // error
} as { [key in Omit<ThemeType, 'followTheSystem'>]: Theme };

```

```ts
import { Q, parseQ, qToString } from '@/utils'
import { useCallback, useMemo } from 'react'
import { useQueryArgs } from '@/hooks/useQueryArgs'

export function useQ() {
    const { query, updateQuery } = useQueryArgs()
    const qStr = useMemo(() => query.q ?? '', [query])
    const q = useMemo(() => parseQ(qStr), [qStr])

    return {
        q,
        updateQ: useCallback(
            (newQ: Q) => {
                updateQuery({
                    q: qToString({
                        ...q,
                        ...newQ,
                    }),
                })
            },
            [q, updateQuery]
        ),
        replaceQ: useCallback(
            (newQ: Q) => {
                updateQuery({
                    q: qToString(newQ),
                })
            },
            [updateQuery]
        ),
        clearQ: useCallback(() => {
            updateQuery({
                q: undefined,
            })
        }, [updateQuery]),
    }
}
```

```ts
<FormItem
  name='confirm_new_password'
  label={t('confirm password')}
  validators={[
      async () => {
          if (values && values.new_password !== values.confirm_new_password) {
              throw new Error(t('password not match'))
          }
      },
  ]}
>
  <Input type='password' />
</FormItem>
```


```ts
import { StatefulTooltip } from 'baseui/tooltip'
import React from 'react'
import ReactTimeAgo from 'react-time-ago'
import { formatTimestampDateTime } from '@/utils/datetime'
import i18n from '@/i18n'

export interface ITimeProps {
    time: number
    style?: React.CSSProperties
}

export default function Time({ time, style }: ITimeProps) {
    return (
        <StatefulTooltip placement='bottom' content={() => formatTimestampDateTime(time)}>
            <ReactTimeAgo style={style} date={new Date(time)} timeStyle='round' locales={i18n.languages as string[]} />
        </StatefulTooltip>
    )
}

```


```ts

export interface ITextProps {
    children: React.ReactNode
    size?: 'small' | 'medium' | 'large'
    style?: React.CSSProperties
}

const fontSizeMap: { [k in Exclude<ITextProps['size'], undefined>]: string } = {
    small: '12px',
    medium: '14px',
    large: '16px',
}

```


```ts
/* eslint-disable react/jsx-props-no-spreading */
import React from 'react'
import { Skeleton } from 'baseui/skeleton'
import { Pagination } from 'baseui/pagination'
import { usePage } from '@/hooks/usePage'
import { useStyletron } from 'baseui'
import { IPaginationProps } from '@/interfaces/IPaginationProps'

export interface IGridProps<T> {
    isLoading?: boolean
    items: T[]
    onRenderItem: (item: T) => JSX.Element
    paginationProps?: IPaginationProps
}

export default function Grid<T>({ isLoading = false, items, onRenderItem, paginationProps }: IGridProps<T>) {
    const [page, setPage] = usePage()

    const [, theme] = useStyletron()

    return (
        <div
            style={{
                paddingTop: 20,
            }}
        >
            <div>
                {isLoading ? (
                    <Skeleton rows={3} animation />
                ) : (
                    <div
                        style={{
                            display: 'grid',
                            gridTemplateColumns: 'repeat(auto-fit, minmax(260px, 400px))',
                            gap: 20,
                        }}
                    >
                        {items.map((item, idx) => (
                            <div
                                key={idx}
                                style={{
                                    position: 'relative',
                                    padding: 10,
                                    border: `1px solid ${theme.borders.border200.borderColor}`,
                                    borderRadius: 2,
                                    boxShadow: theme.lighting.shadow400,
                                }}
                            >
                                {onRenderItem(item)}
                            </div>
                        ))}
                    </div>
                )}
            </div>
            {paginationProps && (
                <div
                    style={{
                        display: 'flex',
                        alignItems: 'center',
                        marginTop: 20,
                    }}
                >
                    <div
                        style={{
                            flexGrow: 1,
                        }}
                    />
                    <Pagination
                        size='mini'
                        numPages={
                            paginationProps.total !== undefined && paginationProps.count !== undefined
                                ? Math.floor(paginationProps.total / paginationProps.count) + 1
                                : 0
                        }
                        currentPage={
                            paginationProps.start !== undefined && paginationProps.count !== undefined
                                ? Math.floor(paginationProps.start / paginationProps.count) + 1
                                : 0
                        }
                        onPageChange={({ nextPage }) => {
                            if (paginationProps.onPageChange) {
                                paginationProps.onPageChange(nextPage)
                            }
                            if (paginationProps.afterPageChange) {
                                setPage({
                                    ...page,
                                    start: (nextPage - 1) * page.count,
                                })
                                paginationProps.afterPageChange(nextPage)
                            }
                        }}
                    />
                </div>
            )}
        </div>
    )
}
```


```ts
import React from 'react'
import { strToCPUMilliCores } from '@/utils'
import { Input, InputProps } from 'baseui/input'

interface ICPUResourceInputProps {
    value?: string
    onChange?: (value: string) => void
    overrides?: InputProps['overrides']
}

export const CPUResourceInput = ({ value, onChange, overrides }: ICPUResourceInputProps) => {
    const milliCores = strToCPUMilliCores(value)
    const n = milliCores === 0 ? '' : String(milliCores)

    return (
        <Input
            overrides={overrides}
            type='number'
            value={n}
            min={0}
            onChange={(e) => {
                const v = (e.target as HTMLInputElement).value
                if (!v || v === '0') {
                    onChange?.('')
                    return
                }
                onChange?.(`${v}m`)
            }}
            endEnhancer='m'
        />
    )
}

```

```ts
type UserMenuProps = Omit<
  MenuProps,
  "id" | "role" | "open" | "anchorOrigin" | "transformOrigin"
> & {
  onChangeTheme: () => void;
};
```