# Strapi application

This application demostrates how to add a confirmation prompt to your edit view in the content manager

## Rationale

- First you extend the Content Manager plugin by creating a directory called `content-manager` in the extensions folder
- The file we're interested in is called `EditViewDataManagerProvider` create a new file in the content-manager folder called `admin/src/containers/EditViewDataManagerProvider/index.js`. Copy the called from [here](https://github.com/strapi/strapi/blob/077741b03725cc981bc984399d553df8a12d89d0/packages/strapi-plugin-content-manager/admin/src/containers/EditViewDataManagerProvider/index.js#L15) to that file so that they are the same.
- Find the `handleSubmit` function. This function is responsible for submitting the form after a change has been made.
- We will add a confirmation dialog before the execution of the submission it should look like this:

```javascript
const handleSubmit = useCallback(
  async (e) => {
    e.preventDefault();
    let errors = {};

    // First validate the form
    try {
      await yupSchema.validate(modifiedData, { abortEarly: false });

      const formData = createFormData(modifiedData);

      if (isCreatingEntry) {
        onPost(formData, trackerProperty);
      } else {
        // ask if the user is sure they want to save
        let conrirm = window.confirm("Are you sure you want to save");
        if (confirm) {
          onPut(formData, trackerProperty);
        }
      }
    } catch (err) {
      console.error("ValidationError");
      console.error(err);

      errors = getYupInnerErrors(err);
    }

    dispatch({
      type: "SET_FORM_ERRORS",
      errors,
    });
  },
  [
    createFormData,
    isCreatingEntry,
    modifiedData,
    onPost,
    onPut,
    trackerProperty,
    yupSchema,
  ]
);
```

- Rebuild your admin by running `npm run build -- --clean` or `yarn build --clean`
- Restart your server
