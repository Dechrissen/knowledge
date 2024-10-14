# Client-side validation

## using Bootstrap

- we can check the validity of form submissions before sending them to our server using Bootstrap

in the HTML file ...
```html
<!-- Add the `required` attribute to each <input> field that needs to be required -->
<!-- and on the outer <form>, add the `novalidate` attribute so that the browser doesn't do the validation -->
<!-- we instead want Bootstrap to do it -->
<!-- The form also needs the class, from our JS below, `validated-form` -->
<form action="/campgrounds" method="POST" novalidate class="validated-form">
    <input class="form-control" type="text" id="title" name="campground[title]" required>
</form>

<!-- This JS to make the Bootstrap validation work needs to be in the file as well (or in the boilerplate template) -->
 <script>
    // Example starter JavaScript for disabling form submissions if there are invalid fields
(function () {
  'use strict'

  // Fetch all the forms we want to apply custom Bootstrap validation styles to
  const forms = document.querySelectorAll('.validated-form')

  // Loop over them and prevent submission
  Array.from(forms)
    .forEach(function (form) {
      form.addEventListener('submit', function (event) {
        if (!form.checkValidity()) {
          event.preventDefault()
          event.stopPropagation()
        }

        form.classList.add('was-validated')
      }, false)
    })
})()
</script>
```