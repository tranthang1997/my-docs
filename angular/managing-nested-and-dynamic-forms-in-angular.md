# Quản lý các Nested và Dynamic Forms trong Angular

Angular Reactive Forms cung cấp cho chúng ta khả năng to lớn với những API mạnh mẽ của nó. Trong bài hướng dẫn nhanh này mình sẽ giải thích các phần tử phần chính
củan form trong Angular và cách kết hợp chúng, lồng chúng(nest) và tự động tạo chúng trong hầu hết mọi tình huống.

## AbstractControl
Đầu tiên, điều quan trọng là bạn phải biết về AbstractControl, một lớp mà hầu hết các phần tử trong form của Angular sẽ kế thừa từ nó. Nó có nhiều thuộc tính 
quản lý mọi thứ từ trạng thái hợp lệ(validity state) đến parent element có thể là gì và các phương thức cho phép chúng ta đánh dấu trạng thái điều khiển 
(touched,untouched, dirty, etc), enable/disable điều khiển, lấy giá trị, đặt giá trị...Có rất nhiều thứ trong class này:

```
abstract class AbstractControl {
  constructor(validator: ValidatorFn, asyncValidator: AsyncValidatorFn)
  value: any
  validator: ValidatorFn | null
  asyncValidator: AsyncValidatorFn | null
  parent: FormGroup | FormArray
  status: string
  valid: boolean
  invalid: boolean
  pending: boolean
  disabled: boolean
  enabled: boolean
  errors: ValidationErrors | null
  pristine: boolean
  dirty: boolean
  touched: boolean
  untouched: boolean
  valueChanges: Observable
  statusChanges: Observable
  updateOn: FormHooks
  root: AbstractControl
  setValidators(newValidator: ValidatorFn | ValidatorFn[]): void
  setAsyncValidators(newValidator: AsyncValidatorFn | AsyncValidatorFn[]): void
  clearValidators(): void
  clearAsyncValidators(): void
  markAsTouched(opts: { onlySelf?: boolean; } = {}): void
  markAsUntouched(opts: { onlySelf?: boolean; } = {}): void
  markAsDirty(opts: { onlySelf?: boolean; } = {}): void
  markAsPristine(opts: { onlySelf?: boolean; } = {}): void
  markAsPending(opts: { onlySelf?: boolean; emitEvent?: boolean; } = {}): void
  disable(opts: { onlySelf?: boolean; emitEvent?: boolean; } = {}): void
  enable(opts: { onlySelf?: boolean; emitEvent?: boolean; } = {}): void
  setParent(parent: FormGroup | FormArray): void
  abstract setValue(value: any, options?: Object): void
  abstract patchValue(value: any, options?: Object): void
  abstract reset(value?: any, options?: Object): void
  updateValueAndValidity(opts: { onlySelf?: boolean; emitEvent?: boolean; } = {}): void
  setErrors(errors: ValidationErrors, opts: { emitEvent?: boolean; } = {}): void
  get(path: string | (string | number)[]): AbstractControl | null
  getError(errorCode: string, path?: string | (string | number)[]): any
  hasError(errorCode: string, path?: string | (string | number)[]): boolean
}
```

## FormControl

Yếu tố cơ bản của việc xây dựng các form Angular là FormControl. Đây là một lớp đại diện cho các phần tử đầu vào trên một trang có tên mà bạn có thể 
đã quen. Bất kỳ phần thông tin nào chúng ta muốn thu thập trong một form, cho dù đó là phần tử input, select, dropdown hay tùy chỉnh đều cần phải có FormControl
đại diện. Derective [formControl] được sử dụng để liên kết phần tử đầu vào trong DOM với FormControl tương ứng của nó.

```
import { Component, OnInit } from '@angular/core';
import { FormControl } from '@angular/forms'

@Component({
  selector: 'app-basic',
  template: `
     <input type="text" [formControl]="name">
  `
})
export class BasicComponent implements OnInit {
  public name = new FormControl('your name here');

  constructor() { }

  ngOnInit() { }
}
```
FormControls có thể được khởi tạo bằng một giá trị, chẳng hạn như "your name here" trong ví dụ trên và trạng thái enabled/disables và đặt bất kỳ trình validators
nào cần thiết.

## FormGroups

FormGroup là lớp cho phép chúng ta nhóm một số formcontrol lại với nhau. Nó cũng kế từ từ lớp AbstractControl, nghĩa là chúng ta có thể theo dõi tính hợp lệ và
giá trị của tất cả các FormControl trong một FormGroup cùng nhau. Điều này cho phép chúng tôi dễ dàng quản lý toàn bộ form của mình.
Derective [formGroup] liên kết FormGroup với một phần tử DOM.

```
import { Component, OnInit } from '@angular/core';
import { FormControl, FormGroup, Validators } from '@angular/forms';

let emailRegex = "^[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+.[a-zA-Z0-9-.]+$";

@Component({
  selector: 'app-formgroup',
  template: `
  <form [formGroup]="userForm" (ngSubmit)="onSubmit()">
    <label>
      First name:
      <input type="text" formControlName="firstName">
    </label>
    <label>
      Last name:
      <input type="text" formControlName="lastName">
    </label>
    <label>
      Email:
      <input type="text" formControlName="email">
    </label>
    <button [disabled]="!userForm.valid" type="submit">submit</button>
  </form>
  `
})
export class FormgroupComponent implements OnInit {
  public userForm = new FormGroup({
    firstName: new FormControl('', {validators: Validators.required}),
    lastName: new FormControl('', {validators: Validators.required}),
    email: new FormControl('', {validators: Validators.pattern(emailRegex)})
  });

  constructor() { }

  ngOnInit() { }

  onSubmit() {
    console.log(this.userForm.value);
  }
}
```

## FormArray

FormArray là một lớp tổng hợp các FormControls thành một mảng, tương tự như FormGroup tạo một đối tượng từ FormControls. FormArrays có thể có các 
controls được đẩy vào hoặc bị xóa khỏi chúng tương tự như cách bạn thao tác một mảng trong vanilla JS và cung cấp cho chúng tôi rất nhiều sức mạnh
và tính linh hoạt khi tạo các form động và lồng nhau.

```
import { Component, OnInit } from '@angular/core';
import { FormControl, FormGroup, FormArray, Validators } from '@angular/forms';

let emailRegex = "^[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+.[a-zA-Z0-9-.]+$";

@Component({
  selector: 'app-formarray',
  template: `
   <form [formGroup]="usersForm" (ngSubmit)="onSubmit()">
    <ng-container *ngFor="let userFormGroup of usersForm.controls; let i = index">
      <div [formGroup]="userFormGroup">
        <label>
          First name:
          <input type="text" formControlName="firstName">
        </label>
        <label>
          Last name:
          <input type="text" formControlName="lastName">
        </label>
        <label>
          Email:
          <input type="text" formControlName="email">
        </label>
      </div>
    </ng-container>
    <button type="submit">console log form value</button>
  </form>
  `
})
export class FormarrayComponent implements OnInit {
  public usersForm = new FormArray([
    new FormGroup({
      firstName: new FormControl('user 1', {validators: Validators.required}),
      lastName: new FormControl('', {validators: Validators.required}),
      email: new FormControl('', {validators: Validators.pattern(emailRegex)})
    }),
    new FormGroup({
      firstName: new FormControl('user 2', {validators: Validators.required}),
      lastName: new FormControl('', {validators: Validators.required}),
      email: new FormControl('', {validators: Validators.pattern(emailRegex)})
    })
  ]);

  constructor() { }

  ngOnInit() { }

  onSubmit() {
    console.log(this.usersForm.value);
  }Trong ví dụ này, chúng tôi đang sử dụng vòng lặp ngFor để lặp qua userForm.controls, vì userForm là FormArray
}
```

Trong ví dụ này, chúng ta đang sử dụng vòng lặp ngFor để lặp qua `userForm.controls`, vì `userForm` là FormArray. Trong FormArray này là các controls,
vì vậy khi chúng ta lặp qua các control, chúng ta cần đảm bảo sử dụng derective [formGroup] để liên kết mỗi phần tử DOM lặp lại với bản sao `FormGroup`
tương ứng của nó.

## FormBuilder
Việc nhập lặp đi lặp lại `new FormControl(''), new FormGroup({}), và new FormArray([])` có thể trở nên hơi tẻ nhạt, đặc biệt khi tạo các form lớn hơn.
May mắn thay Angular có cú pháp viết tắt mà chúng ta có thể sử dụng nhờ vào lớp `FormBuilder`.

```
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

let emailRegex = "^[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+.[a-zA-Z0-9-.]+$";

@Component({
  selector: 'app-formbuilder',
  template: `
  <form [formGroup]="usersForm" (ngSubmit)="onSubmit()">
    <ng-container *ngFor="let userFormGroup of usersForm.controls.users.controls; let i = index">
      <div [formGroup]="userFormGroup">
        <label>
          First name:
          <input type="text" formControlName="firstName">
        </label>
        <label>
          Last name:
          <input type="text" formControlName="lastName">
        </label>
        <label>
          Email:
          <input type="text" formControlName="email">
        </label>
      </div>
    </ng-container>
    <button type="submit">console log form value</button>
  </form>
  `
})
export class FormbuilderComponent implements OnInit {
  public usersForm: FormGroup;

  constructor(private fb: FormBuilder) { }

  ngOnInit() {
    this.usersForm = this.fb.group({
      date: this.fb.control(new Date()),
      users: this.fb.array([
        this.fb.group({
          firstName: [{value: 'user 1', disabled: false}, Validators.required],
          lastName: [{value: '', disabled: false}, Validators.required],
          email: [{value: '', disabled: false}, Validators.pattern(emailRegex)]
        }),
        this.fb.group({
          firstName: [{value: 'user 2', disabled: false}, Validators.required],
          lastName: [{value: '', disabled: false}, Validators.required],
          email: [{value: '', disabled: false},  Validators.pattern(emailRegex)]
        })
      ])
    })
  }

  onSubmit() {
    console.log(this.usersForm.value);
  }
}
```
Bây giờ chúng ta đã hiểu về các phần mà chúng ta có thể tạo form, hãy cùng xem xét việc xây dựng một ví dụ form phức tạp hơn.

## Tự động tạo và xóa FormControls & FormGroups

Giả sử chúng ta muốn tạo một form cho phép người dùng tạo không giới hạn số lượng người dùng. Form này sẽ cần có khả năng cho phép người dùng thêm FormGroups mới
để tạo thêm người dùng nếu cần, cũng như xóa FormGroups mà họ không muốn. Chúng tôi sử dụng `FormArray` để giữ một FormGroups cho mỗi người dùng mà chúng tôi
muốn tạo. Để làm điều này, chúng ta có thể sử dụng các phương thức của FormArray:
  * Phương thức `insert` có hai tham số, chỉ số để chèn và điều khiển để chèn.
  * Phương thức `removeAt`, lấy chỉ mục của controls để xóa nó.
  
```
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, FormArray, Validators } from '@angular/forms';

let emailRegex = "^[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+.[a-zA-Z0-9-.]+$";

@Component({
  selector: 'app-addformgroups',
  template: `
   <form [formGroup]="usersForm" (ngSubmit)="onSubmit()">
    <ng-container *ngFor="let userFormGroup of usersForm.controls.users.controls; let i = index">
      <div [formGroup]="userFormGroup">
        <label>
          First name:
          <input type="text" formControlName="firstName">
        </label>
        <label>
          Last name:
          <input type="text" formControlName="lastName">
        </label>
        <label>
          Email:
          <input type="text" formControlName="email">
        </label>
        <label>
          <button (click)="removeFormControl(i)">remove formGroup</button>
        </label>
      </div>
    </ng-container>
  </form>
  <button (click)="addFormControl()">add new user formGroup</button>
  `
})
export class AddformgroupsComponent implements OnInit {
  public usersForm: FormGroup;

  constructor(private fb: FormBuilder) { }

  ngOnInit() {
    this.usersForm = this.fb.group({
      date: this.fb.control(new Date()),
      users: this.fb.array([
        this.fb.group({
          firstName: ['user 1', Validators.required],
          lastName: ['', Validators.required],
          email: ['', Validators.pattern(emailRegex)]
        }),
        this.fb.group({
          firstName: ['user 2', Validators.required],
          lastName: ['', Validators.required],
          email: ['',  Validators.pattern(emailRegex)]
        })
      ])
    })
  }

  removeFormControl(i) {
    let usersArray = this.usersForm.controls.users as FormArray;
    usersArray.removeAt(i);
  }

  addFormControl() {
    let usersArray = this.usersForm.controls.users as FormArray;
    let arraylen = usersArray.length;

    let newUsergroup: FormGroup = this.fb.group({
      firstName: ['', Validators.required],
      lastName: ['', Validators.required],
      email: ['', Validators.pattern(emailRegex)]
    })

    usersArray.insert(arraylen, newUsergroup);
  }
}
```
Bây giờ chúng ta có một form tự động thêm và loại bỏ các FormGroups để cho phép người dùng tạo bao nhiêu người dùng tùy thích. Với sự hiểu biết sâu sắc về 
FormControl, FormGroup và FormArray, chúng ta có thể tạo bất kỳ cấu trúc biểu mẫu nào để đáp ứng nhu cầu gửi dữ liệu của mình.

### Tài liệu tham khảo
https://www.bitovi.com/blog/managing-nested-and-dynamic-forms-in-angular


