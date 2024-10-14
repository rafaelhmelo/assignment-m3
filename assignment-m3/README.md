# assignment-m3

To start, run "npm run serve"

Notes:

- This is an extremely basic setup. The focus of the 8h was on meeting the requirements.

If given more time, I'd do the following adjustments:

- Before anything: If more time was allowed, the first thing I would do is add more comments to the validation functions. Comments are always good to have, but I usually implement first and comment later.
- Refactor the code and split it between multiple files. For example, validation code in one separated file, and separated component files for the email and phone number lists, which are dynamic.
- Add a proper cs compiler like grunt or webpack instead of adding raw css. Setting those up take time, and I'd rather invest it into meeting the requirements.
- Improving styles with tailwind, for example, which would have made styling easier.

About assignment 2:

- Would probably spend more time finding a better way to add the clipping "dent" in the ticket using css. The current way is an illusion.
- Would, like in the previous task, use a css compiler tool, but since I didn't have any readily setup and that would consume the allowed 8h time, I went with pure css for a task this size.
- Would add some hover effects and/or an idle animation to add a "shining" effect to the ticket that loops every X seconds.