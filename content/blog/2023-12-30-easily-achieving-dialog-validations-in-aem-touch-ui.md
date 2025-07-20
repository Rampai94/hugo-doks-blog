---
title: Easily achieving Dialog Validations in AEM Touch UI
description: ""
summary: ""
date: 2023-12-28T15:10:10+02:00
lastmod: 2023-12-28T15:10:22+02:00
draft: false
weight: 50
categories: []
tags: []
contributors: []
pinned: false
homepage: false
seo:
  title: "Dialog Validations in AEM Touch UI" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Today, I will be listing my observations with dialog validations in AEM touch UI. Versions tested: AEM 6.3/6.4.

You all might have come across scenarios where you need to apply a regex check for email inside dialog text-field or simply check if the field values aren’t blank. Blank checks can be put by using required property in AEM fields supporting the same.


Validation to check if the entered text is a valid URL.
But in case of complex validations, the approach that I had been following was to create a custom client-library folder for each component and writing duplicate js for each text-field to catch those using ID/class/name property.

I knew I was doing something wrong but didn’t know how to fix the approach! Not anymore, since I came across this new way of dialog validations using foundation-validation.

Let me list down the reasons why I fell in love with this approach first of all:

Central client-library folder for all dialog validations.
No duplicate js required and fetching of fields can be done in a more efficient way that applies across all fields of the same type. e.g. all text-fields can be validated using the same js file by simply adding few properties.
How do we do it? 💭
Create a generic client-library folder named “touch-dialog-validation” for the project with categories -> cq.authoring.dialog or any custom client-library categories that you need. In case cq.authoring.dialog is used it is invoked by default but in case you are using custom clientlib categories then you need to include the data-sly-call in your template or component.
Include js file for writing validations.
Create a property named “validation” with the value registered with foundation.validation.validator in validations.js file. (In our case, the txt-validate property.)
Create “granite:data” node (nt:unstructured) under node where you need validation and add additional attributes as properties that might be needed for writing validation logic. (In our case, the pattern property.) Note: it need not always be validation property, we can use granite:class or granite:id as selectors. In this case the selector inside the validations.js will need to be changed accordingly.

touch-dialog-validation with type cq:ClientLibraryFolder and validations.js included in it.
If field within multi-field, say text-field needs to be validated, same can be written and by simply using respective selector on field node validation can be achieved.

Script example (The code is mentioned in this SO answer by me):

AEM 6.2 touch UI validation on Text field
In my AEM touch ui dialog, i have a text field. In that text field, author can enter the data like this. "[$]" or "{$}"…
stackoverflow.com

Screenshots:

Example 1: Check for data not blank and contains URL.

CRX:



Include property validation (this is selector) and create a node granite:data for additional properties being used in script.
Output:



Blank check with additional check for URL Pattern.
Example 2: Check for data not blank and contains alphabets for textfield inside coral multifield. Multifield Resource Type : granite/ui/components/coral/foundation/form/multifield

CRX:



Include property validation (this is selector) and create a node granite:data for additional properties being used in script.
Output:



Blank check with additional check for mandatory Alphabets.
Ready. Set. 🏃

Hope this helped you! If it did, please do make a point to shower your love by sending some 👏