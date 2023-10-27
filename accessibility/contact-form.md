https://www.w3.org/WAI/tutorials/forms/
https://webaim.org/techniques/forms/
https://www.nngroup.com/articles/form-design-placeholders/

witch fields are mandatory
https://www.w3.org/WAI/tutorials/forms/instructions/


required attribute is read by screen reader in HTML5 or is possible to use aria-required=true
https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-required


     <section>
                  <h2 class="mb-3 h2">Send us an email!</h2>
                  <p>All fields marked with an asterisk (*) are required.</p>
                  <form class="contact_form">
                     <div class="form-group">
                        <label>
                           Your Name <span aria-hidden="true" style="color:red;">*</span>
                           <input type="text" class="form-control" required>
                        </label>
                     </div>
                     <div class="form-group">
                        <label>
                           Email <span aria-hidden="true" style="color:red;">*</span>
                           <input type="email" class="form-control" required>
                        </label>
                     </div>
                     <label>
                        Your Message
                        <span aria-hidden="true" style="color:red;">*</span>
                        <textarea class="form-control" id="msg" required></textarea>
                     </label>
                     <div class="input-group">
                        <button type="submit" class="btn btn-primary mt-3">Send Message</button>
                     </div>
                  </form>
               </section>
