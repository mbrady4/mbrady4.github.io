# Forms

Definitions of common form elements:

- The HTML <form> element represents a document section that contains interactive controls for submitting information to a web server.
- The HTML <input> element is used to create interactive controls for web-based forms in order to accept data from the user.
    - Make checkbox default to checked with: `<input type="checkbox" name="nameOfChoice" value="1" checked>`
- The HTML <label> element represents a caption for an item in a user interface.

Standard HTML form in a React Application: 

```jsx
<div className="App">
      <form>
        <label htmlFor='fnameInput'>First Name</label>
        <input
        name='fname'
        id='fnameInput'
        type='text'
        maxLength='15'
        placeHolder='First Name'
       /><br/>

      <label htmlfor='favVehicleSelect'>Fav Vehicle</label>
       <select>
         <option values='1'>Cars</option>
         <option values='2'>Trucks</option>
         <option values='3'>Planes</option>
        </select><br/>

       <label htmlFor='isHappyInput'>Are you happy?</label>
       <input type='checkbox' id='isHappyInput' name='isHappy' /><br />

       <input type='submit' />
      </form>
    </div>
```

## onChange

The `onChange` handler on an input captures the typing event. The event object stores the new value from the input. We get access to the typing event from onChange, because the event holds the value of the input. Because of this, we have access to what our user’s input from the event object! This pathway is vital if we want to manage input values in react state, rather than through the DOM.

```jsx
// inline
<input onChange={event => console.log(event)} />

// function defined somewhere else
const logEvent = event => {
  console.log(event);
}

...

<input onChange={logEvent} />
```

### Example using onChange to state to reflect input in real-time

```jsx
export default function App() {
  const [fname, setFname] = useState('')
  const [favVehicle, setFavVehicle] = useState('');
  const [isHappy, setIsHappy] = useState(false);

  return (
    <div className="App">
      <h3>The name is {fname || 'unkown'}</h3>
      <h3>The fav vehicle is {favVehicle || 'unkown'}</h3>
      <h3>The mood is {isHappy ? 'happy' : 'unhappy'}</h3>

      <form>
        <label>
          first name: <input onChange={event => {
            setFname(event.target.value);
          }} id='fname' type='text' />
       </label><br/>

       <label>
         fav vehicle: <select onChange={event => {
           setFavVehicle(event.target.value);
         }} id='favVehicle'>
           <option />
          <option>Cars</option>
          <option>Trucks</option>
          <option>Planes</option>
        </select>
        </label><br/>

        <label>
          happy: <input onChange={event => {
            setIsHappy(event.target.checked);
          }} id='isHappy' type='checkbox' />
        </label>

       {/* <input type='submit' /> */}
      </form>
    </div>
  );
}
```

[modest-black-s1orj](https://codesandbox.io/s/modest-black-s1orj)

## Handling Submits

If there’s only one <button> inside a <form>, the form will know to fire the function attached to its onSubmit listener. The form is also the element causing the default refresh of our page, and, as such, it’s the only thing that can prevent it, so we have to grab the event object from the form.

[busy-bird-ywyo8](https://codesandbox.io/s/busy-bird-ywyo8?file=/src/App.js)

```jsx
import React, { useState } from "react";
import "./styles.css";
import axios from 'axios';

export default function App() {
  const [name, setName] = useState('');
  const [age, setAge] = useState('');
  const onSubmit = event => {
    event.preventDefault();
    axios.get(`name=${name}&age=${age}`);
  }
  const onNameChange = event => setName(event.target.value);
  const onAgeChange = event => setAge(event.target.value);

  return (
    <div className="App">
      <form onSubmit={onSubmit}>
        <label>
          name: <input 
            onChange={onNameChange}
            id='nameInput' 
            type='text' 
          />
       </label>
       <label>
         age: <input 
               onChange={onAgeChange}
                id='ageInput' 
                type='number' 
              />
      </label>
          <button>send</button>
        </form>

      <h5>The name is {name} and the age is {age}</h5>
    </div>
  );
}
```

## Handling form values with a single object in state

[Handling Form Values with Single State Object](https://codesandbox.io/s/handling-form-values-with-single-state-object-hjy8i)

```jsx
import React, { useState } from "react";
import "./styles.css";

export default function App() {
  const [formData, setFormData] = useState({
    fname: '',
    lname: '',
    favToy: ''
  });

  const onInputChange = event => {
    setFormData({
      ...formData,
      [event.target.name]: event.target.value,
    });
  };

  return (
    <div className="App">
      <form>
        <label>
          First Name:
          <input name='fname' onChange={onInputChange} />
        </label>
        <label>
          Last Name:
          <input name='lname' onChange={onInputChange} />
        </label>
        <label>
          Favorite Toy:
          <input name='favtoy' onChange={onInputChange} />
        </label>
      </form>
    </div>
  );
}
```

- **The Spread Operator**

    When declaring bar we take all the key/value pairs inside foo (in this case, there’s only one) and “spread” them out (copy them) inside a brand new object literal. This gives us the behavior we expected to see in the first place.

    ```jsx
    let foo = { key: "value" };

    let bar = { ...foo };

    console.log(foo); //  {key: "value"}
    console.log(bar); //  {key: "value"}

    foo.key = "change";

    console.log(foo); //  {key: "change"}
    console.log(bar); //  {key: "value"}
    ```

    ## Controlled Inputs

    Using the `value` attribute forces the text inside the input field to correspond to the string assigned to it - in this case, “Hi Lambda!”. Hardcoding it like this makes the input rather useless because now the user can’t change the text. But, what if, instead of hardcoding the value attribute, we passed it a dynamic value from our state.

    [controlled inputs](https://codesandbox.io/s/controlled-inputs-vp27n)

    ```jsx
    import React, { useState } from "react";
    import "./styles.css";

    export default function App() {
      const [formValues, setFormValues] = useState({
        fname: '', lname: '',
      });

      const onValueChange = event => {
        if (/^[a-zA-Z]+$/.test(event.target.value)) {
            setFormValues({
              ...formValues,
              [event.target.name]: event.target.value
            });
        };
      };
      const onFormSubmit = event => {
        event.preventDefault();
        alert(`submitting ${formValues.lname}, ${formValues.fname}`);
      }
      return (
        <form className="Component" onSubmit={onFormSubmit}>
          <input
            value={formValues.fname}
            placeholder='Enter First Name'
            onChange={onValueChange}
            name='fname'
          />
          <input
            value={formValues.lname}
            placeholder='Enter Last Name'
            onChange={onValueChange}
            name='lname'
          />
          <input type='submit'/>
        </form>
      );
    }
    ```

    In HTML, form elements such as `<input>`, `<textarea>`, and `<select>` typically maintain their own state and update it based on user input. In React, mutable state is typically kept in the state property of components, and only updated with `[setState()](https://reactjs.org/docs/react-component.html#setstate)`.

    We can combine the two by making the React state be the “single source of truth”. Then the React component that renders a form also controls what happens in that form on subsequent user input. An input form element whose value is controlled by React in this way is called a “controlled component”.

    ## Yup for form validation

    Yup has built-in methods to validate email addresses, passwords, strings, numbers, and more. 

    `npm install --save yup`

    we simply declare a schema with `let schema = yup.string()`; and test our schema on a new line with await `schema.isValid('hello world');`. This would return “true” since ‘hello world’ is a string.

    `import * as Yup from "yup";`

    [jquense/yup](https://github.com/jquense/yup)

    ## Post requests with Axios

    ```jsx
    const sentData = { data: "Hello World!" };

    axios
      .post("https://yourdatabaseurlgoeshere.com", sentData)
      .then(res => {
        console.log(res.data); // Data was created successfully and logs to console
      })
      .catch(err => {
        console.log(err); // There was an error creating the data and logs to console
      });
    ```

    A POST request, such as the one above, might return a response (or res) like the following:

    ```jsx
    {
      error: false,
      data: { data: "Hello World!" },
      message: "Your data was successfully created."
    }
    ```

    This is just a rough example, and every server works differently. You will need to initially console.log() the server’s response to find out what kind of data you’re receiving back in your response.

    1. Create a new state to handle the Post. This is where to store data on a valid form submit.

    ```jsx
    // new state to set our post request too. So we can console.log and see it.
      const [post, setPost] = useState([]);

      useEffect(() => {
        formSchema.isValid(formState).then(valid => {
          // console.log(valid);
          setButtonDisabled(!valid);
        });
      }, [formState]);
    ```

    2. Post to a database from a form submit event handler.

    ```jsx
    // this handles what happens when we submit the form. We want to prevent the default
      //form submission from the browser and control what happens when we submit.
      const formSubmit = e => {
        e.preventDefault();
        console.log("submitted!");
        axios
          .post("https://reqres.in/api/users", formState)
          .then(res => {
            setPost(res.data); // get just the form data from the REST api
            console.log("success", res);
          })
          .catch(err => console.log(err.response));
      };
    ```

    ## Example

    [Adv-Form-4.27.20](https://codesandbox.io/s/adv-form-42720-x63ti?file=/src/components/Form.js:0-4291)

    ```jsx
    import React, { useState, useEffect } from "react";
    import * as yup from "yup"; // DOCS: https://github.com/jquense/yup
    import axios from "axios";

    export default function Form() {
      // Just to handle our POST request
      const [post, setPost] = useState([])

      // managing state for our form inputs
      const [formState, setFormState] = useState({
        name: '',
        email: '',
        motivation: '',
        positions: '',
        terms: ''
      });

      // Manage state of checkbox
      const [isButtonDisabled, setIsButtonDisabled] = useState(true);
      
      // managing state for errors in a form submission
      const [errors, setErrors] = useState({
        name: '',
        email: '',
        motivation: '',
        positions: '',
        terms: ''
      });

      // create schema for form validation
      const formSchema = yup.object().shape({
        name: yup.string().required("Name is a required field"),
        email: yup.string().email("Must be a valid email address").required(),
        terms: yup.boolean().oneOf([true], "please agree to terms."),
        positions: yup.string(),
        motivation: yup.string().required('Please explain motivation.')
      });

      // Modify state for errors (the messages) based on validation
      // This sets the errors states for rendering
      const validateChange = e => {
        yup
          .reach(formSchema, e.target.name)
          .validate(e.target.value)
          .then( valid => {
            setErrors({...errors, [e.target.name]: ""})
          })
          .catch( err => {
            setErrors({ ...errors, [e.target.name]: err.errors[0]})
          });
      };

      // Triggers validation when FormState is modified
      useEffect( () => {
        // Is valid will compare formState to formSchema and return a boolean
        formSchema.isValid(formState).then(valid => {
          // This console.log did not work well
          console.log("valid?", valid);
          setIsButtonDisabled(!valid);
        });
      }, [formState]);

      // onSubmit function
      const formSubmit = event => {
        event.preventDefault();
        axios.post('https://reqres.in/api/users', formState)
          .then(response => {
            setPost(response.data);
            setFormState({
              name: '',
              email: '',
              motivation: '',
              positions: '',
              terms: ''
            });
          })
          .catch(err => console.log(err.response));
      };

      // onChange function
      const inputChange = event => {
        console.log('input changed!', event.target.value);
        event.persist()
        const newFormData = {
          ...formState,
          [event.target.name]: event.target.type === 'checkbox' ? 
                               event.target.checked : 
                               event.target.value
        }
        validateChange(event);
        setFormState(newFormData);
      };

      return (
        <form onSubmit={formSubmit}>
          <label htmlFor='name'>
            Name
            <input type='text' name='name' onChange={inputChange} value={formState.name} />
            {/* render the render message if it exists */}
            {errors.name.length > 0 ? <p className='error'>{errors.name}</p> : null }
          </label>
          <label htmlfor='email'>
            Email
            <input type='email' name='email' onChange={inputChange} value={formState.email}/>
            {errors.email.length > 0 ? <p className='error'>{errors.email}</p> : null }
          </label>
          <label htmlFor='motivation'>
            Why would you like to volunteer?
            <textarea name='motivation' onChange={inputChange} value={formState.motivation}/>
            {errors.motivation.length > 0 ? <p className='error'>{errors.motivation}</p> : null}
          </label>
          <label htmlFor='positions'>
            What would you like to help with?
            <select id='positions' name='positions' onChange={inputChange}>
              <option value='Newsletter'>Newsletter</option>
              <option value='Yard Work'>Yard Work</option>
              <option value='Admin Work'>Admin</option>
              <option value='Tabling'>Tabling</option>
            </select>
          </label>
          <label htmlFor='terms' className='terms'>
            <input type='checkbox' name='terms' checked={formState.terms} onChange={inputChange} />
            Terms & Conditions
          </label>
          {/* This just is for testing and seeing the JSON response */}
          <pre>{JSON.stringify(post, null, 2)}</pre>
          <button disabled={isButtonDisabled} type='submit'>Submit</button>
        </form>
      );
    }
    ```

    ## Advanced Form's: A Recipe

    1. **Build the structure of the form**
    2. **Create a state object to control the values of the form**
        - UseState and and onChange function (create helper function to handle set state values to form values on change)
    3. **Create a schema for form validation** 
        - Create a function to validateChanges based on formSchema. Create a state object to manage error messages
        - Create a useEffect to trigger validation when the Form Values states is modified
    4. **Build an on Form Submit function that takes the Form Values state and submits it via a POST request**
        - Create a state object to store the response
        - Reset Form Values to blank/initial values

## Controlling Forms with Custom Hooks

[custom-hooks-webpt15](https://codesandbox.io/s/custom-hooks-webpt15-t5tfc?file=/src/components/SignupForm.js)