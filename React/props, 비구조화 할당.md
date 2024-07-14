
컴포넌트에서 props를 보다 간결하고 명확하게 사용하기 위해 비구조화 할당을 사용한다.

우선 아래 예시 코드를 보자

```javascript
import React from 'react';
import PropTypes from 'prop-types';

const MyComponent = (props) => {
    return (
        <div>
            <h1>name: {props.name}</h1>
            <h1>children: {props.children}</h1>
        </div>
    );
}

MyComponent.defaultProps = {
    name: 'default name'
};

MyComponent.propTypes = {
    name: PropTypes.string,
    children: PropTypes.node
};

export default MyComponent;
```

여기서 'props' 라는 ***객체***를 사용하여 name과 children에 접근하는데 매번 props. 을 사용하여 접근해야하는 귀찮음이 있다. (접근자로 매번 객체에서 추출해야함)


하지만 비구조화 할당을 사용하면 다음과 같이 코드를 작성 할 수 있다.

---

```js
import React from 'react';
import PropTypes from 'prop-types';

const MyComponent = ({ name, children }) => {
    return (
        <div>
            <h1>name: {name}</h1>
            <h1>children: {children}</h1>
        </div>
    );
}

MyComponent.defaultProps = {
    name: 'default name'
};

MyComponent.propTypes = {
    name: PropTypes.string,
    children: PropTypes.node
};

export default MyComponent;
```

코드가 더 간결해지고 명확하게 name, children을 할당해 사용 할 수 있다.
props 객체에서 가져오지만!!! 각 prop을 가져와 사용해 더 명확하게 사용하는 느낌을 받았다.

언어 마다 데이터 객체에 대한 접근 방식이 비슷하지만 내가 접해본 언어 중에서는
가장 깔끔하고 접근자를 매번 사용하지 않아도 되는 점이 흥미로웠다.

그렇지만 나중에는 어떤 객체를 참조하는지 혼란스럽거나 prop의 수가 많아지면 더 코드가 복잡해져 가독성이 떨어질 것이다.

변수의 수가 그리많지 않다면 깔끔한 코드를 작성 할 수 있겠지만 의도가 명확하지 않고 불분명해 지기 때문에 되도록 지양해서 사용하도록 하자