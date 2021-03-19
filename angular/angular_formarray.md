Trong bài viết này, mình sẽ tìm hiểu mọi thứ bạn cần biết về cấu trúc Angular FormArray, có sẵn trong Angular Reactive Forms. Chúng ta sẽ tìm hiểu chính xác Angular FormArray là gì,
sự khác biệt đối với FormGroup bình thường là gì, khi nào sử dụng nó và tại sao.

Mình sẽ đưa ra một ví dụ về trường hợp sử dụng phổ biến sẽ khó thực hiện nếu không có FormArray: một bảng có thể chỉnh sửa tại chỗ với nhiều form control trên mỗi dòng,
nơi người dùng có thể thêm hoặc xóa các hàng theo yêu cầu.

## Vậy Angular FormArray là gì?
Trong Angular Reactive Forms, mọi form đều có mô hình được xác định bằng cách sử dụng các FormControl và FormGroup API hoặc cách khác
là sử dụng FormBuilder API ngắn gọn hơn mà mình sẽ sử dụng trong suốt hướng dẫn này.
Trong hầu hết các trường hợp, tất cả các trường của một form đều được biết rõ từ trước và vì vậy chúng ta có thể xác định
một mô hình tĩnh cho một form bằng cách sử dụng FormGroup:

```
form = this.fb.group({
    title: ['', {
        validators: [
            Validators.required,
            Validators.minLength(5),
            Validators.maxLength(60)
        ],
        asyncValidators: [courseTitleValidator(this.courses)],
        updateOn: 'blur'
    }],
    releasedAt: [new Date(), Validators.required],
    category: ['BEGINNER', Validators.required],
    downloadsAllowed: [false, Validators.requiredTrue],
    longDescription: ['', [Validators.required, Validators.minLength(3)]]
});
```

Như chúng ta có thể thấy, bằng cách sử dụng FormGroup, chúng ta có thể xác định một nhóm các form control có liên quan, các giá trị ban đầu của chúng và quy tắc xác
thực form, đồng thời cung cấp tên thuộc tính cho từng trường của form.
Điều này có thể xảy ra vì đây là một form tĩnh với một số trường được xác định trước, đây là trường hợp phổ biến nhất khi nói đến form.

Nhưng còn những trường hợp khác nâng cao hơn nhưng vẫn thường xuyên gặp phải trong đó formnăng động hơn nhiều và không phải tất cả các trường trong form đều được biết
trước (nếu có)?

Bạn hãy tưởng tượng một form động trong đó các form control được người dùng thêm hoặc xóa vào biểu mẫu, tùy thuộc vào tương tác của nó với giao diện người dùng.
Một ví dụ sẽ là một form được xây dựng hoàn toàn động theo dữ liệu đến từ backend!

Một ví dụ khác phổ biến hơn về form động sẽ là một bảng có thể chỉnh sửa tại chỗ, nơi người dùng có thể thêm hoặc xóa các dòng chứa nhiều form control có
thể chỉnh sửa:

![alt text](https://angular-university.s3-us-west-1.amazonaws.com/blog-images/angular-form-array/angular-form-array-example.png)

Trong form động này, người dùng có thể thêm hoặc xóa các form control mới cho form bằng cách sử dụng các nút Thêm và Xóa. Mỗi lần người dùng nhấp vào nút Thêm, một hàng
bài học mới sẽ được thêm vào biểu mẫu có chứa hai form control mới. Chúng ta sẽ thực hiện ví dụ này trong hướng dẫn này bằng cách sử dụng FormArray. Nhưng câu hỏi chính
là: Tại sao biểu mẫu này không thể được triển khai bằng FormGroup?

## Sự khác biệt giữa FormArray và FormGroup là gì?
Không giống như ví dụ ban đầu của chúng ta, nơi chúng ta đã xác định mô hình form bằng cách sử dụng FormGroup thông qua lệnh gọi tới `fb.group()` API, trong trường hợp
bảng có thể chỉnh sửa tại chỗ, chúng tôi không biết trước số lượng form control.

Và điều này là do chúng ta không thể biết trước số hàng mà bảng sẽ có. Người dùng có thể thêm một số hàng không xác định bằng cách sử dụng nút thêm và thậm chí anh ta có
thể xóa chúng giữa chừng bằng cách sử dụng nút xóa bài học.

Nhưng chúng ta có thể xác định mô hình form cho bảng có thể chỉnh sửa tại chỗ này bằng cách sử dụng FormArray để thay thế.
FormArray cũng giống như FormGroup, cũng là một vùng chứa các form control, nó tổng hợp các giá trị và trạng thái hợp lệ của các thành phần con của nó. Nhưng không giống
như FormGroup, vùng chứa FormArray không yêu cầu chúng ta phải biết tất cả các form control trước, cũng như tên của chúng.

Trên thực tế, FormArray có thể có số lượng form control không xác định, bắt đầu từ 0 Sau đó, các form control có thể được thêm và xóa động tùy thuộc vào cách người
dùng tương tác với giao diện người dùng. Mỗi form control sau đó sẽ có một vị trí số trong mảng các form control, thay vì một tên duy nhất.

Có thể thêm hoặc xóa các form control khỏi form bất kỳ lúc nào trong thời gian chạy bằng FormArray API.

##FormArray API
Đây là các method được sử dụng phổ biến nhất hiện có trong FormArray API:
  * controls: Đây là một mảng chứa tất cả các controls là một phần của mảng
  * length: Đây là tổng chiều dài của mảng
  * at(index): Trả về form control tại một vị trí nhất định
  * push(control): Thêm control mới vào cuối mảng
  * removeAt(index): Loại bỏ một control tại một vị trí nhất định của mảng
  * getRawValue(): Nhận các giá trị của tất cả các form control, thông qua thuộc tính control.value của mỗi control

##Tạo bảng có thể chỉnh sửa tại chỗ bằng FormArray
Trước tiên, hãy xác định một mô hình reactive form, sử dụng FormBuilder API:

```
@Component({
  selector: 'form-array-example',
  templateUrl: 'form-array-example.component.html',
  styleUrls: ['form-array-example.component.scss']
})
export class FormArrayExampleComponent {

    form = this.fb.group({
        ... other form controls ...
        lessons: this.fb.array([])
    });

    constructor(private fb:FormBuilder) {}
  
    get lessons() {
      return this.form.controls["lessons"] as FormArray;
    }
}
```
Như chúng ta có thể thấy, chúng tôi đã không thêm bất kỳ control nào khác vào form của mình, ngoại trừ chính bảng dữ liệu có thể chỉnh sửa.

Điều này chỉ để giữ cho ví dụ đơn giản, nhưng không có gì ngăn cản chúng ta thêm bất kỳ thuộc tính nào khác nếu cần.

Trong trường hợp của chúng ta, tất cả các control bên trong bảng có thể chỉnh sửa sẽ nằm bên trong FormArray instance, được xây dựng bằng `fb.array()` API.
Ban đầu, FormArray trống và không chứa form control, có nghĩa là bảng có thể chỉnh sửa ban đầu là trống hoàn toàn.

Mình đã thêm vào component một getter cho thuộc tính lessons , để làm cho nó dễ dàng truy cập cá thể FormArray một cách đơn giản và an toàn về kiểu.
Ngoài ra, hãy lưu ý trong ảnh chụp màn hình bảng có thể chỉnh sửa được hiển thị trước đó, mỗi hàng trong bảng chứa hai control: trường tiêu đề bài học và
trường cấp độ bài học (hoặc độ khó).

Mình muốn các trường này là một phần của parent form của component này và ảnh hưởng đến trạng thái hợp lệ của nó. Nếu lỗi xảy ra ở tiêu đề của một
trong các bài học, thì toàn bộ form sẽ được coi là không hợp lệ.

Đối với điều này, chúng ta sẽ tạo một FormGroup cho mỗi hàng bảng và chúng ta sẽ thêm vào nó hai trường biểu mẫu, với trình form control validators của riêng chúng.

##Tự động thêm các controls vào FormArray
Để thêm các hàng mới vào bảng, chúng ta sẽ cần một nút `Add Lesson` trong mẫu thành phần của chúng ta:

```
<button mat-mini-fab (click)="addLesson()">
  <mat-icon class="add-course-btn">add</mat-icon>
</button>
```
Khi nút này được nhấp vào, chúng ta sẽ kích hoạt mã sau:

```
addLesson() {
  const lessonForm = this.fb.group({
      title: ['', Validators.required],
      level: ['beginner', Validators.required]
  });

  this.lessons.push(lessonForm);
}
```

Như chúng ta thấy, để thêm một hàng vào bảng, trước tiên chúng ta tạo mô hình form của một biểu mẫu đơn giản với chỉ hai trường của mỗi hàng: tên bài học và độ khó.
Sau đó, chúng ta lấy hàng FormGroup trong bài học này, bản thân nó cũng là mộtform control  và chúng ta thêm nó vào vị trí cuối cùng của FormArray bằng cách
sử dụng `push `.

Hãy nhớ rằng, chúng ta có thể thêm bất kỳ form control nào vào FormArray và điều này cũng bao gồm các form groups.

##Tự động xóa các control khỏi FormArray

Nếu bạn nhận thấy trong ảnh chụp màn hình bảng có thể chỉnh sửa được hiển thị trước đó, mỗi hàng bài học đều có biểu tượng xóa được liên kết với nó, người dùng có thể
sử dụng để xóa toàn bộ hàng bài học.

Đây là quá trình xử lý nhấp chuột cho nút này:
```
deleteLesson(lessonIndex: number) {
    this.lessons.removeAt(lessonIndex);
}
```
Để xóa một hàng bài học, tất cả những gì chúng ta phải làm là sử dụng `removeAt` để xóa FormGroup tương ứng khỏi FormArray, tại một chỉ mục nhất định.

Lưu ý rằng trong cả nút thêm bài học và loại bỏ bài học, tất cả những gì chúng ta phải làm để thêm hoặc bớt một hàng là thêm hoặc bớt các form control khỏi form. Và điều này
là do mô hình giao diện người dùng của chúng ta được xây dựng bằng cách lặp qua các phần tử FormArray và bằng cách thêm các form control cho phù hợp.

##formArrayName directive

Để kết thúc ví dụ của chúng ta, bây giờ chúng ta sẽ hiển thị mã đầy đủ củacomponent bảng có thể chỉnh sửa và xem xét mẫu:

```
@Component({
  selector: 'form-array-example',
  templateUrl: 'form-array-example.component.html',
  styleUrls: ['form-array-example.component.scss']
})
export class FormArrayExampleComponent {

    form = this.fb.group({
        ... other form controls ...
        lessons: this.fb.array([])
    });

    constructor(private fb:FormBuilder) {}
  
    get lessons() {
      return this.form.controls["lessons"] as FormArray;
    }

    addLesson() {
      const lessonForm = this.fb.group({
        title: ['', Validators.required],
        level: ['beginner', Validators.required]
      });
      this.lessons.push(lessonForm);
    }

    deleteLesson(lessonIndex: number) {
      this.lessons.removeAt(lessonIndex);
    }
}
```
Và đây là template:

```
<h3>Add Course Lessons:</h3>
<div class="add-lessons-form" [formGroup]="form">
    <ng-container formArrayName="lessons">
        <ng-container *ngFor="let lessonForm of lessons.controls; let i = index">
            <div class="lesson-form-row" [formGroup]="lessonForm">
                <mat-form-field appearance="fill">
                    <input matInput
                           formControlName="title"
                           placeholder="Lesson title">
                </mat-form-field>
                <mat-form-field appearance="fill">
                    <mat-select formControlName="level"
                            placeholder="Lesson level">
                        <mat-option value="beginner">Beginner</mat-option>
                        <mat-option value="intermediate">Intermediate</mat-option>
                        <mat-option value="advanced">Advanced</mat-option>
                    </mat-select>
                </mat-form-field>
                <mat-icon class="delete-btn" (click)="deleteLesson(i)">
                    delete_forever</mat-icon>
            </div>
        </ng-container>
    </ng-container>
  
    <button mat-mini-fab (click)="addLesson()">
        <mat-icon class="add-course-btn">add</mat-icon>
    </button>

</div>

```

Bây giờ chúng ta hãy phân tích những gì đang xảy ra:
  * Bảng có thể chỉnh sửa này được triển khai bằng cách sử dụng các Angular Material components thường được sử dụng.
  * Chúng ta đang áp dụng formGroup directive cho một vùng chứa biểu mẫu và liên kết nó với parent form của component.
  * Điều này có nghĩa là parent form sẽ chứa tất cả các control con của form, bao gồm mọi control  của mọi hàng do người dùng tạo.
  * Bên trong parent form, chúng ta áp dụng formArrayName directive, liên kết phần tử vùng chứa với thuộc tính lessons của form.
  * Directive này là thứ cho phép FormArray theo dõi các giá trị và trạng thái hợp lệ của các thành phần con của nó.
  * Sau đó, chúng ta đang sử dụng ngFor để lặp qua các form control của FormArray.
  * Mọi control của FormArray là một form group, chứa hai form control (title và level).
  * Sau đó, chúng ta sử dụng directive formGroup để xác định một nested form cho mọi hàng trong bảng, chứa hai form control bài học.
  * Sau đó, chúng ta sử dụng directive formControlName để liên kết các trường bài học với mẫu, giống như chúng tôi thường làm trong bất kỳ reactive form nào.
  
Như chúng ta có thể thấy, form bảng có thể chỉnh sửa của chúng tôi chỉ đơn giản là một form có danh sách các nested child forms, một form trên mỗi hàng.
Và mỗi form trên 1 hàng của bảng chứa hai form control bên trong nó.

##Tài liệu tham khảo
https://blog.angular-university.io/angular-form-array/

  
