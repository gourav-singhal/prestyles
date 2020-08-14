# Prestyle
A simple little package that makes writing cross-platform responsive RN styles a bit tidier.


## Installation
`yarn add prestyle`

## Usage (createStyles)

1. Import the library

    `import createStyles from 'prestyle';`

2. Use this function where you would normally use `StyleSheet.create()`;

3. This library allows you to add sub objects to your style objects that contain platform specific styles.

    ```jsx harmony
    const styles = createStyles({
       button: {
           backgroundColor: 'red',
           web: {
               backgroundColor: 'blue'
           }   
       }
   });
    ```
   
   Consume your styles as normal:
   
   `<Button style={styles.button}/>`
   
   The supported platform keys are:
    - web (only used on web)
    - native (used on ios and android)
    - ios (used on ios)
    - android (used on android)
    
4. This library also allows you to pass params to your style objects

    ```jsx harmony
   const styles = createStyles({
       button: (color) => ({
           fontSize: 12,
           color: color,
           native: {
               fontSize: 10
           }
       })
    });
    ```
   
   Consume:
   
   `<Button style={styles.button('red')}/>`
   
## Usage (responsiveStyles)

1. Configure your breakpoints at top level `App.js` or wherever.
    
   ```jsx harmony
   import {setBreakpoints} from 'prestyle';
   
   setBreakpoints({
       desktop: 1024, // min width,
       tablet: 768, // min width
       mobile: 0
   });
    ```
2. Import the style creator and hook

    ```jsx harmony
   import {
       createResponsiveStyles, 
       useResponsiveStyles
   } from 'prestyle';
    ```
   
3. Create styles with breakpoint sub styles

    ```jsx harmony
    const styles = createResponsiveStyles({
       container: {
           backgroundColor: 'white', // Will be shown if no matching breakpoint
           desktop: {
               backgroundColor: 'blue' // Will be show on > 1024 widths
           },
           tablet: {
               backgroundColor: 'red'
           },
           mobile: {
               backgroundColor: 'green'
           }   
       }
    });
    ```
   
4. Use the `useResponsiveStyles` hook to consume styles that will dynamically change based on matching breakpoints

    ```jsx harmony
    const Foo = () => {
       const [responsiveStyles] = useResponsiveStyles(styles);
   
       return (
           <View style={responsiveStyles.container}/>
       )  
   }
    ```
  
5. The `useResponsiveStyles` hook accepts two params.
    1. The required `styles` object generated from `createResponsiveStyles`
    2. An optional custom breakpoint object that will overwrite whatever was set at top-level with `setBreakpoints`
  
6. The `useResponsiveStyles` hook returns an array with 3 values:
    1. The parsed styles
    2. A boolean letting you know if the styles are ready (in cased you experience any flicker between the default styles, and the matched styles on load).
    3. An updated screen with value if you need it for anything
