tags u need
nav
ul 
li
a
 
 
 <nav id="primary-nav" aria-label="Main">
                  <div class="main-nav bg-primary">
            <ul class="container list-group bg-primary">
               <li>
                  <a aria-current="page" href="#"
                     class="list-group-item list-group-item-action bg-primary border-0 active">Home </a>
               </li>
               <li>
                  <a href="#" class="list-group-item list-group-item-action bg-primary border-0 ">New Patients</a>
               </li>
               <li>
                  <a href="#" class="list-group-item list-group-item-action bg-primary border-0">Testimonials</a>
               </li>
               <li>
                  <a href="#" class="list-group-item list-group-item-action bg-primary border-0">Hospital Gallery</a>
               </li>
               <li>
                  <a href="#" class="list-group-item list-group-item-action bg-primary border-0">Services</a>
               </li>
               <li>
                  <a href="#" class="list-group-item list-group-item-action bg-primary border-0">Appointments</a>
               </li>
               <li>
                  <a href="#" class="list-group-item list-group-item-action bg-primary border-0">Contact us</a>
               </li>
            </ul>
         </div>
      </nav>

not use title='xxx' on a tag because screenreadres are gonna read the title value and the text inside the tag so probably ripening them self
https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-current
manage selection by aria-current
#primary-nav ul li a[aria-current="page"]{
    font-weight: bold;
    border-bottom:1px solid orange;
}