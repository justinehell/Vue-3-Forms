<template>
  <div>
    <h1>Create an Event</h1>
    <!-- No neet to add @submit.prevent because Vee-Validate does this for us -->
    <form @submit="submit">
      <BaseSelect
        label="Select a category"
        :options="categories"
        v-model="category"
        :error="categoryError"
      />

      <h3>Name & describe your event</h3>
      <BaseInput
        label="Title"
        v-model="title"
        :error="titleError"
        type="text"
      />

      <BaseInput
        label="Description"
        v-model="description"
        :error="descriptionError"
        type="text"
      />

      <h3>Where is your event?</h3>
      <BaseInput
        label="Location"
        v-model="location"
        :error="locationError"
        type="text"
      />

      <h3>Are pets allowed?</h3>
      <BaseRadioGroup
        v-model="pets"
        :error="petsError"
        name="pets"
        :options="[
          { value: 1, label: 'Yes' },
          { value: 0, label: 'No' },
        ]"
      />

      <h3>Extras</h3>
      <div>
        <BaseCheckbox
          label="Catering"
          v-model="catering"
          :error="cateringError"
        />
      </div>

      <div>
        <BaseCheckbox label="Live music" v-model="music" :error="musicError" />
      </div>

      <div>
        <BaseButton type="submit" class="-fill-gradient" something="else">
          Submit
        </BaseButton>
      </div>
    </form>
  </div>
</template>

<script>
import { useField, useForm } from 'vee-validate';
export default {
  data() {
    return {
      categories: [
        'sustainability',
        'nature',
        'animal welfare',
        'housing',
        'education',
        'food',
        'community',
      ],
    };
  },
  setup() {
    const required = (value) => {
      const requiredMessage = 'This field is required';
      if (value === undefined || value === null) return requiredMessage;
      if (!String(value).length) return requiredMessage;
      return true;
    };
    const minLength = (number, value) => {
      if (String(value).length < number)
        return 'Please type at least ' + number + ' characters';
      return true;
    };
    const anything = () => {
      return true;
    };

    const validationSchema = {
      category: required,
      title: (value) => {
        const req = required(value);
        if (req !== true) return req;

        const min = minLength(3, value);
        if (min !== true) return min;

        return true;
      },
      description: required,
      location: undefined,
      pets: anything,
      catering: anything,
      music: anything,
    };

    // handleSubmit is provided by Vee-Validate, that handles checking that the form is valid before submitting it for us
    const { handleSubmit } = useForm({
      validationSchema,
    });

    const { value: category, errorMessage: categoryError } =
      useField('category');
    const { value: title, errorMessage: titleError } = useField('title');
    const { value: description, errorMessage: descriptionError } =
      useField('description');
    const { value: location, errorMessage: locationError } =
      useField('location');

    // For the next 3 fields, useField shows 3 parameters : model, validation method, config object - set property initialValue
    const { value: pets, errorMessage: petsError } = useField(
      'pets',
      undefined, // set to 'anything' inside 'validationSchema'
      { initialValue: 1 }
    );
    const { value: catering, errorMessage: cateringError } = useField(
      'catering',
      undefined, // set to 'anything' inside 'validationSchema'
      { initialValue: false }
    );
    const { value: music, errorMessage: musicError } = useField(
      'music',
      undefined, // set to 'anything' inside 'validationSchema'
      { initialValue: false }
    );

    // Our submit form logic
    const submit = handleSubmit((values) => {
      // this callback function will be only executed if the user tries to submit a valid form
      // and it will not be executed if any errors are present in our form

      // The 'values' param of the callback is an Object that will contain all the property/value pairs of the form’s data,
      // matching the exact structure that we declared on our validationsSchema.
      console.log('submit', values);
    });

    return {
      category,
      categoryError,
      title,
      titleError,
      description,
      descriptionError,
      location,
      locationError,
      pets,
      petsError,
      catering,
      cateringError,
      music,
      musicError,

      submit,
    };
  },
};
</script>
