# Airbnb မှ React/JSX Style Guide

*A mostly reasonable approach to React and JSX*

## မာတိကာ

  1. [အခြေခံ စည်းမျဉ်းများ](#basic-rules)
  1. [Class vs `React.createClass` vs stateless](#class-vs-reactcreateclass-vs-stateless)
  1. [Naming](#naming)
  1. [Declaration](#declaration)
  1. [Alignment](#alignment)
  1. [Quotes](#quotes)
  1. [Spacing](#spacing)
  1. [Props](#props)
  1. [Refs](#refs)
  1. [Parentheses](#parentheses)
  1. [Tags](#tags)
  1. [Methods](#methods)
  1. [Ordering](#ordering)
  1. [`isMounted`](#ismounted)

## basic-rules

  - Only include one React component per file. File တခုတွင် React Component တခုသာပါရှိပါစေ
  - သို့သော် Stateless သို့မဟုတ် ရိုးရှင်းတဲ့ Components(https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) များမှာတော့တခုထက်မကပါဝင်နိုင်ပါတယ်။eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
    
  - JSX Syntax ကိုသာအမြဲသုံးပေးပါ။.
  - JSX File မဟုတ် တဲ့ တခြားဖိုင်တွေနဲ့(Eg. javascript File) သင့် app ကိုစမယ်ဆိုရင် `React.createElement` ကိုမသုံးရပါဘူး။
  

## Class vs `React.createClass` vs stateless

   - အကယ် လို့ သင့် Component ထဲမှာ state သို့ မဟုတ် refs တွေရှိနေမယ် ဆိုရင် `React.createClass` ထက်စာရင် `class extends React.Component` ကိုသုံးသင့်ပါတယ် သင့်မှာ mixing တွေသုံးဖို့ ခိုင်လုံတဲ့အကြောင်းပြချက်မရှိခဲ့ရင်။eslint: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md) [`react/prefer-stateless-function`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-stateless-function.md)

    ```jsx
    // မသုံးသင့်သောပုံစံ
    const Listing = React.createClass({
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    });

    // သုံးသင့်သောပုံစံ
    class Listing extends React.Component {
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    }
    ```

    ပြီးတော့ သင့်ဆီမှာ state သို့မဟုတ် refs တွေမရှိခဲ့ရင် class ကိုသုံးတာထက်စာရင် ပုံမှန် functions(arrow function မဟုတ်ပါ)များကိုသာသုံးသင့်ပါတယ်။

    ```jsx
    // မသုံးသင့်သောပုံစံ
    class Listing extends React.Component {
      render() {
        return <div>{this.props.hello}</div>;
      }
    }

    // မသုံးသင့်သောပုံစံ (relying on function name inference is discouraged)
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    );

    // သုံးသင့်သောပုံစံ
    function Listing({ hello }) {
      return <div>{hello}</div>;
    }
    ```

## Naming

  - **Extensions**: React components တွေအတွက်သာ `.jsx` ကိုသုံးပါ။  
  - **Filename**: File နာမည်များကို PascalCase သာသုံးပါ။ E.g., `ReservationCard.jsx`.
  - **Reference Naming**: PascalCase ကို React components အတွက်သုံးပြီး တော့ component ထဲမှာပါတဲ့ method တွေကိုတော့ camelCase သုံးပါ။. eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

    ```jsx
    // မသုံးသင့်သောပုံစံ
    import reservationCard from './ReservationCard';

    // သုံးသင့်သောပုံစံ
    import ReservationCard from './ReservationCard';

    // မသုံးသင့်သောပုံစံ
    const ReservationItem = <ReservationCard />;

    // သုံးသင့်သောပုံစံ
    const reservationItem = <ReservationCard />;
    ```

  - **Component Naming**: Use the filename as the component name. For example, `ReservationCard.jsx` should have a reference name of `ReservationCard`. However, for root components of a directory, use `index.jsx` as the filename and use the directory name as the component name:
  - **Component Naming**: file နာမည်များကို component နာမည်များအဖြစ်သုံးပေးပါ။ ဥပမာ `ReservationCard.jsx` မှာ reference နာမည် က `ReservationCard` ပဲဖြစ်သင့်ပါတယ်။ သို့သော် Directory(ဖိုလ်ဒါ)တခုရဲ့ root component တွေကိုတော့ File နာမည် ကို `index.jsx` လို့သုံးသင့်ပြီး Directory(ဖိုလ်ဒါ) နာမည် ကို component နာမည်အဖြစ်သုံးသင့်ပါတယ်။  

    ```jsx
    // မသုံးသင့်သောပုံစံ
    import Footer from './Footer/Footer';

    // မသုံးသင့်သောပုံစံ
    import Footer from './Footer/index';

    // သုံးသင့်သောပုံစံ
    import Footer from './Footer';
    ```

## Declaration

  - components တွေကို နာမည်ပေးဖို့ `displayName` မသုံးရပါ. ထိုအစား  component ကို reference အားဖြင့်သာ နာမည်ပေးသင့်ပါတယ်။

    ```jsx
    // မသုံးသင့်သောပုံစံ
    export default React.createClass({
      displayName: 'ReservationCard',
      // stuff goes here
    });

    // သုံးသင့်သောပုံစံ
    export default class ReservationCard extends React.Component {
    }
    ```

## Alignment

  - Jsx Language အထားအသို အတွက် အောက်ပါ alignment ပုံစံများကိုလိုက်နာပါ။ eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```jsx
    // မသုံးသင့်သောပုံစံ
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // သုံးသင့်သောပုံစံ
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    />

    // if props fit in one line then keep it on the same line
    <Foo bar="bar" />

    // children get indented normally
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    >
      <Quux />
    </Foo>
    ```

## Quotes

  - Always use double quotes (`"`) for JSX attributes, but single quotes for all other JS. eslint: [`jsx-quotes`](http://eslint.org/docs/rules/jsx-quotes)
  - JSX ရဲ့ attributes များကို (`"`) များဖြင့်အမြဲတမ်းသုံးပေးပါ။, သို့သော် (`'`) ကို တခြား JS များမှာသုံးပါ။. eslint: [`jsx-quotes`](http://eslint.org/docs/rules/jsx-quotes)

  > Why? JSX attributes [can't contain escaped quotes](http://eslint.org/docs/rules/jsx-quotes), so double quotes make conjunctions like `"don't"` easier to type.
  > Regular HTML attributes also typically use double quotes instead of single, so JSX attributes mirror this convention.
  
  > ဘာကြောင့်လဲ? JSX attributes [ escaped quotes တွေပါလို့မရပါ](http://eslint.org/docs/rules/jsx-quotes), `"don't"` ကဲ့ သို့သော စာသားများ ကို လွယ်ကူစွာ ရိုက်ရန် double quotes တွေက တွဲဖက်အနေနဲ့ ပြုလုပ်ပေးပါတယ်။
  > ပုံမှန် HTML attributes တွေဟာလည်း များသောအားဖြင့် single quotes အစား double quotes များကိုသာသုံးကြပါတယ်, JSX attributes တွေဟာလဲ ထိုစံနှုန်းကိုယူထားခြင်းဖြစ်ပါတယ်။

    ```jsx
    // မသုံးသင့်သောပုံစံ
    <Foo bar='bar' />

    // သုံးသင့်သောပုံစံ
    <Foo bar="bar" />

    // မသုံးသင့်သောပုံစံ
    <Foo style={{ left: "20px" }} />

    // သုံးသင့်သောပုံစံ
    <Foo style={{ left: '20px' }} />
    ```

## Spacing

  - Always include a single space in your self-closing tag.
  - self-closing tag တွမှာ အမြဲတမ်း space တခုတော့ အမြဲထည့်ပေးပါ။

    ```jsx
    // မသုံးသင့်သောပုံစံ
    <Foo/>

    // မသုံးသင့်သောပုံစံ
    <Foo                 />

    // မသုံးသင့်သောပုံစံ
    <Foo
     />

    // သုံးသင့်သောပုံစံ
    <Foo />
    ```

  - JSX ရဲ့ တွန့်ကွင်း များကို space တွေနဲ့မဖြည့်သင့်ပါ။. eslint: [`react/jsx-curly-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)

    ```jsx
    // မသုံးသင့်သောပုံစံ
    <Foo bar={ baz } />

    // သုံးသင့်သောပုံစံ
    <Foo bar={baz} />
    ```

## Props

  - Always use camelCase for prop names.
  - prop names များကို camelCase သာအမြဲသုံးပေးပါ။

    ```jsx
    // မသုံးသင့်သောပုံစံ
    <Foo
      UserName="hello"
      phone_number={12345678}
    />

    // သုံးသင့်သောပုံစံ
    <Foo
      userName="hello"
      phoneNumber={12345678}
    />
    ```

  - prop ရဲ့ value ဟာ လံုးဝ `true` ဖြစ်မှာသေချာတယ်ဆိုရင် value ကိုမပေးဘဲချန်ထားလို့ရပါတယ်။. eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

    ```jsx
    // မသုံးသင့်သောပုံစံ
    <Foo
      hidden={true}
    />

    // သုံးသင့်သောပုံစံ
    <Foo
      hidden
    />
    ```

  - `<img>` tag တွေမှာ `alt` prop ကိုအမြဲတမ်း ထည့်ပေးပါ. အကယ်လို့ image ကို တင်ပြဖို့အတွက်သုံးမယ်ဆိုရငတော့ `alt` ကို string အလွတ် သို့မဟုတ် `role="presentation"` ကိုထည့်သင့်ပါတယ်။ eslint: [`jsx-a11y/img-has-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-has-alt.md)

    ```jsx
    // မသုံးသင့်သောပုံစံ
    <img src="hello.jpg" />

    // သုံးသင့်သောပုံစံ
    <img src="hello.jpg" alt="Me waving hello" />

    // သုံးသင့်သောပုံစံ
    <img src="hello.jpg" alt="" />

    // သုံးသင့်သောပုံစံ
    <img src="hello.jpg" role="presentation" />
    ```

  - Do not use words like "image", "photo", or "picture" in `<img>` `alt` props. eslint: [`jsx-a11y/img-redundant-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-redundant-alt.md)
  - `<img>` `alt` ရဲ့ props တွေမှာ "image", "photo" or "picture" ကဲ့သို့သာေစကားလံုးများကိုမသုံးရပါ။ eslint: [`jsx-a11y/img-redundant-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-redundant-alt.md)

  > ဘာကြောင့်လဲ? Screenreaders များ မှာ `img` element များကို ပုံ အဖြစ် သိရှိရန် ပြုလုပ်ထားပြီးဖြစ်ပါတယ်။ ထို့ကြောင့် ထိုကဲ့သို့သော အချက်အလက်များကို alt မှာထည့်ဖို့မလိုပါ။

    ```jsx
    // မသုံးသင့်သောပုံစံ
    <img src="hello.jpg" alt="Picture of me waving hello" />

    // သုံးသင့်သောပုံစံ
    <img src="hello.jpg" alt="Me waving hello" />
    ```

  - တိကျသာေအခေါ် အဝေါ် များကိုသာ သုံးပါ [ARIA roles](https://www.w3.org/TR/wai-aria/roles#role_definitions). eslint: [`jsx-a11y/aria-role`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/aria-role.md)

    ```jsx
    // မသုံးသင့်သောပုံစံ - not an ARIA role
    <div role="datepicker" />

    // မသုံးသင့်သောပုံစံ - abstract ARIA role
    <div role="range" />

    // သုံးသင့်သောပုံစံ
    <div role="button" />
    ```

  - element တွေပါ်မှာ `accessKey` မသုံးသင့်ပါ။. eslint: [`jsx-a11y/no-access-key`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-access-key.md)

  > ဘာကြောင့်လဲ? အသုံးပြုသူတွေရဲ့ Screenreaders တွေနဲ့ keyboard(commands) တွဲမှတ်မှုများက တဦးနှင့်တဦး မတူလို့ပါ။ 

  ```jsx
  // မသုံးသင့်သောပုံစံ
  <div accessKey="h" />

  // သုံးသင့်သောပုံစံ
  <div />
  ```

  - array အခန်း နံပတ်များ ကို prop ရဲ့ `key` အဖြစ်မသုံးသင့်ပါ။ မတူညီနိုင်တဲ့ ID(unique ID) များကိုသာသုံးသင့်ပါတယ်။ ([why?](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318))

  ```jsx
  // မသုံးသင့်သောပုံစံ
  {todos.map((todo, index) =>
    <Todo
      {...todo}
      key={index}
    />
  )}

  // သုံးသင့်သောပုံစံ
  {todos.map(todo => (
    <Todo
      {...todo}
      key={todo.id}
    />
  ))}
  ```

## Refs

  - `ref` များကို callback ဖြင့်သုံးပါ. eslint: [`react/no-string-refs`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-string-refs.md)

    ```jsx
    // မသုံးသင့်သောပုံစံ
    <Foo
      ref="myRef"
    />

    // သုံးသင့်သောပုံစံ
    <Foo
      ref={(ref) => this.myRef = ref}
    />
    ```

## Parentheses

  - တကြောင်းထက်ပိုသာေ JSX tags များကို လက်သည်းကွင်းတွင်ထည့်ပါ။ eslint: [`react/wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/wrap-multilines.md)

    ```jsx
    // မသုံးသင့်သောပုံစံ
    render() {
      return <MyComponent className="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // သုံးသင့်သောပုံစံ
    render() {
      return (
        <MyComponent className="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );
    }

    // သုံးသင့်သောပုံစံ, တကြောင်းတည်းသော JSX tag
    render() {
      const body = <div>hello</div>;
      return <MyComponent>{body}</MyComponent>;
    }
    ```

## Tags

  - child tag မရှိတဲ့ tag များကို self-close tage ပုံစံပြောင်းရေးပါ။. eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

    ```jsx
    // မသုံးသင့်သောပုံစံ
    <Foo className="stuff"></Foo>

    // သုံးသင့်သောပုံစံ
    <Foo className="stuff" />
    ```

  - သင့် component မှာ တကြောင်းထက် ပိုတဲ့ properties တွေရှိနေခဲ့ရင် ၄င်း tag ကို နောက် တကြောင်းဆင်းပြီး ပိတ်ပေးပါ။ eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```jsx
    // မသုံးသင့်သောပုံစံ
    <Foo
      bar="bar"
      baz="baz" />

    // သုံးသင့်သောပုံစံ
    <Foo
      bar="bar"
      baz="baz"
    />
    ```

## Methods

  - Use arrow functions to close over local variables.

    ```jsx
    function ItemList(props) {
      return (
        <ul>
          {props.items.map((item, index) => (
            <Item
              key={item.key}
              onClick={() => doSomethingWith(item.name, index)}
            />
          ))}
        </ul>
      );
    }
    ```

  - constructor ထဲမှာသာ render method ရဲ့ event handlers များ ကို တွဲ(bind) ပေးပါ။  eslint: [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)

  > Why? A bind call in the render path creates a brand new function on every single render.
  > ဘာကြောင့်လဲ? render method ကိုတခါတခါ ခေါ်တိုင်းမှာ bind သည် လည်းပဲ အသစ်ထပ်မံခေါ်ခံရလို့ဖြစ်ပါတယ်။

    ```jsx
    // မသုံးသင့်သောပုံစံ
    class extends React.Component {
      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv.bind(this)} />
      }
    }

    // သုံးသင့်သောပုံစံ
    class extends React.Component {
      constructor(props) {
        super(props);

        this.onClickDiv = this.onClickDiv.bind(this);
      }

      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv} />
      }
    }
    ```

  - `_` ကို React Component အတွင်းမှာရှိတဲ့ methods တွေမှာ မသံုးရပါ။

    ```jsx
    // မသုံးသင့်သောပုံစံ
    React.createClass({
      _onClickSubmit() {
        // do stuff
      },

      // other stuff
    });

    // သုံးသင့်သောပုံစံ
    class extends React.Component {
      onClickSubmit() {
        // do stuff
      }

      // other stuff
    }
    ```

  - Be sure to return a value in your `render` methods. eslint: [`require-render-return`](https://github.com/yannickcr/eslint-plugin-react/pull/502)
  - `render` method တွေမှာ return value ပါဝင်ပါစေ။ eslint: [`require-render-return`](https://github.com/yannickcr/eslint-plugin-react/pull/502)

    ```jsx
    // မသုံးသင့်သောပုံစံ
    render() {
      (<div />);
    }

    // သုံးသင့်သောပုံစံ
    render() {
      return (<div />);
    }
    ```

## Ordering

  - Ordering for `class extends React.Component`:
  - `class extends React.Component` ရဲ့ method အထားအသို အစီအစဥ်:

  1. optional `static` methods
  1. `constructor`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *clickHandlers or eventHandlers* like `onClickSubmit()` or `onChangeDescription()`
  1. *getter methods for `render`* like `getSelectReason()` or `getFooterContent()`
  1. *Optional render methods* like `renderNavigation()` or `renderProfilePicture()`
  1. `render`

  - `propTypes`, `defaultProps`, `contextTypes`, စသည်ဖြင့်... ဘယ်လို သတ်မှတ်မလဲ

    ```jsx
    import React, { PropTypes } from 'react';

    const propTypes = {
      id: PropTypes.number.isRequired,
      url: PropTypes.string.isRequired,
      text: PropTypes.string,
    };

    const defaultProps = {
      text: 'Hello World',
    };

    class Link extends React.Component {
      static methodsAreOk() {
        return true;
      }

      render() {
        return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>
      }
    }

    Link.propTypes = propTypes;
    Link.defaultProps = defaultProps;

    export default Link;
    ```

  - Ordering for `React.createClass`: eslint: [`react/sort-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/sort-comp.md)

  1. `displayName`
  1. `propTypes`
  1. `contextTypes`
  1. `childContextTypes`
  1. `mixins`
  1. `statics`
  1. `defaultProps`
  1. `getDefaultProps`
  1. `getInitialState`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *clickHandlers or eventHandlers* like `onClickSubmit()` or `onChangeDescription()`
  1. *getter methods for `render`* like `getSelectReason()` or `getFooterContent()`
  1. *Optional render methods* like `renderNavigation()` or `renderProfilePicture()`
  1. `render`

## `isMounted`

  - `isMounted` ကိုအသံုးမပြုသင့်တော့ပါ။ eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

  > ဘာကြောင့်လဲ? [`isMounted` is an anti-pattern][anti-pattern], ၄င်း ကို ES6 မှာအသံုးမပြုနိုင်တော့လို့ပါ, နောက်ပြီး၄င်း ကို ဖျက်ဖို့ တရားဝင်စာတင်ထားပြီးဖြစ်ပါတယ်

  [anti-pattern]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html

## Translation

  ဤ JSX/React ရေးသားပုံ လမ်းညွှန်ကို အခြားသော ဘာသာစကားများဖြင့်လည်း ဖတ်ရှုနိုင်ပါသည်။

  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese (Simplified)**: [JasonBoy/javascript](https://github.com/JasonBoy/javascript/tree/master/react)
  - ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [pietraszekl/javascript](https://github.com/pietraszekl/javascript/tree/master/react)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [apple77y/javascript](https://github.com/apple77y/javascript/tree/master/react)
  - ![mm](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Myanmar.png) **Myanmar**: [winhtaikaung/javascript](https://github.com/winhtaikaung/javascript/tree/master/react)

**[⬆ back to top](#မာတိကာ)**
