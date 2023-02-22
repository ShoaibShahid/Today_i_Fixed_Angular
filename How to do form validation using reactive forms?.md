## Problem
...  Angular form validation using Reactive forms   ...

## Environment
- VS Code
## How you fix it
...  We can do form validation in Angular using reactive forms. As Reactive forms has variety of option for forms in Angular. I have used bootstrap to make user error most user friendly  ...

## Solution

## intall bootsrap

``` npm install bootstrap ```
## Add bootstrap css path 

``` in style array of angular.json  ./node_modules/bootstrap/dist/css/bootstrap.min.css ```
## Add Reactive forms module

``` import ReactiveFormsModule into your local module(appModule)   ```
## component TS file

```  
 contactForm: any;
  constructor() {
    this.contactForm = this.initializeForm();
  }

  ngOnInit(): void {
   this.confirmValidator();
  }
  onSubmit() {
    console.log(this.contactForm)
  }

  private initializeForm(){
    return new FormGroup({
      firstname: new FormControl('', Validators.required),
      lastname: new FormControl('', Validators.required),
      email: new FormControl('', [Validators.required, Validators.email]),
      country: new FormControl('', Validators.required),
      password: new FormControl('', [Validators.required, Validators.minLength(6)]),
      confirmPassword: new FormControl('', [Validators.required, Validators.minLength(6)]),
      acceptTerms: new FormControl(false, [Validators.required])
    });
  }

  onReset(){
    this.contactForm.reset();    
  }

  private confirmValidator(){
    this.contactForm.valueChanges.subscribe((form: any) => {
      const password = form.password;
      const confirm = form.confirmPassword;
      if (password !== confirm) {
        this.contactForm.get('confirmPassword').setErrors({ confirmedValidator: true });
      }
    });
  }
   ```

## component HTML file
 ``` 
 <div class="card m-3">
  <h5 class="card-header">Angular form Validation using Reactive Form & bootstrap</h5>
  <div class="card-body">
    <form [formGroup]="contactForm" #form="ngForm" (ngSubmit)="!contactForm.invalid && onSubmit()">
      <div class="form-row">

        <div class="form-group col">
          <label>First Name</label>
          <input type="text" formControlName="firstname" class="form-control"
            [ngClass]="{ 'is-invalid': form.submitted && contactForm.get('firstname')?.errors }" />
          <div *ngIf="contactForm.hasError('required','firstname')" class="invalid-feedback">
            <div>First Name is required</div>
          </div>
        </div>
        <div class="form-group col">
          <label>Last Name</label>
          <input type="text" formControlName="lastname" class="form-control"
            [ngClass]="{ 'is-invalid': form.submitted && contactForm.get('lastname')?.errors }" />
          <div *ngIf="contactForm.hasError('required','lastname')" class="invalid-feedback">
            <div >Last Name is required</div>
          </div>
        </div>
      </div>
      <div class="form-row">

        <div class="form-group col">
          <label>Email</label>
          <input type="text" formControlName="email" class="form-control"
            [ngClass]="{ 'is-invalid': form.submitted && contactForm.get('email')?.errors }" />
          <div *ngIf="contactForm['controls']['email']['errors']" class="invalid-feedback">
            <div *ngIf="contactForm.hasError('required','email')">Email is required</div>
            <div *ngIf="contactForm.hasError('email','email')">String must be a valid email address</div>
          </div>
        </div>

        <div class="form-group col">
          <label>country</label>
          <select class="form-control" name="country" formControlName="country"
            [ngClass]="{ 'is-invalid': form.submitted && contactForm.get('country')?.errors }">
            <option>Select</option>
            <option value="1">Pakistan</option>
            <option value="2">USA</option>
            <option value="3">England</option>
            <option value="4">Singapore</option>
          </select>
          <div *ngIf="contactForm.get('country')?.errors" class="invalid-feedback">
            <div *ngIf="contactForm.hasError('required','country')">country is required</div>
          </div>
        </div>

      </div>

      <div class="form-row">
        <div class="form-group col">
          <label>Password</label>
          <input type="password" formControlName="password" class="form-control"
            [ngClass]="{ 'is-invalid': form.submitted && contactForm.get('password')?.errors }" />
          <div *ngIf="contactForm.get('password')?.errors" class="invalid-feedback">
            <div *ngIf="contactForm.hasError('required','password')">Password is required</div>
            <div *ngIf="contactForm.hasError('minlength','password')">Password must be at least 6
              characters</div>

          </div>
        </div>
        <div class="form-group col">
          <label>Confirm Password</label>
          <input type="password" formControlName="confirmPassword" class="form-control"
            [ngClass]="{ 'is-invalid': form.submitted && contactForm.get('confirmPassword')?.['errors']  }" />
          <div *ngIf="form.submitted && contactForm.get('confirmPassword')?.['errors']" class="invalid-feedback">
            <div *ngIf="contactForm.hasError('required','confirmPassword')">Confirm Password is required
            </div>
            <div *ngIf="contactForm.hasError('minlength','confirmPassword')">Confirm Password must be at least 6
              characters</div>
          </div>
          <div class="invalid-feedback" *ngIf="contactForm.hasError('confirmedValidator','confirmPassword')">
            Passwords don't match
          </div>
        </div>
      </div>

      <div class="form-group form-check">
        <input type="checkbox" formControlName="acceptTerms" id="acceptTerms" class="form-check-input"
          [ngClass]="{ 'is-invalid': form.submitted && contactForm.get('acceptTerms')?.errors }" />
        <label for="acceptTerms" class="form-check-label">Accept Terms & Conditions</label>
        <div
          *ngIf="contactForm.hasError('required','acceptTerms')"
          class="invalid-feedback">Accept Ts & Cs is required</div>
      </div>
      <div class="text-center">
        <button class="btn btn-primary mr-1">Register</button>
        <button class="btn btn-secondary" type="reset" (click)="onReset()">Clear form</button>
      </div>
    </form>
  </div>
</div>

 ```

 So in this case a is-invalid class is attached to invalid controls to show errors
