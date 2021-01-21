## Một vài mẹo với YAML

&nbsp;&nbsp;YAML là định dạng tệp thường được sử dụng để serialization dữ liệu. Có rất nhiều dự án sử dụng tệp YAML để cấu hình, chẳng hạn như
[Docker-compose](https://docs.docker.com/compose/),[pre-commit](https://pre-commit.com/#2-add-a-pre-commit-configuration),
[TravisCI](https://docs.travis-ci.com/user/build-config-yaml), [AWS Cloudformation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-formats.html),
[ESLint](https://eslint.org/docs/user-guide/configuring), [Kubernetes](https://kubernetes.io/docs/concepts/configuration/configmap/#configmaps-and-pods),
[Ansible](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html),và nhiều hơn nữa. Biết các tính năng của YAML sẽ giúp bạn tiếp cận
với tất cả project dễ hơn.
  
&nbsp;&nbsp;Trước tiên, chúng ta hãy đề cập đến những điều cơ bản: YAML là một tập hợp lớn của JSON (source). Mọi tệp JSON hợp lệ cũng là một tệp YAML hợp lệ.
Điều này có nghĩa là bạn có tất cả các kiểu mà bạn mong đợi: Integers, floats, strings, bool, null. Ngoài ra còn sequences, maps. Tùy thuộc vào ngôn ngữ lập
trình của bạn, bạn có thể nói “array” hoặc “list” thay vì sequences và “dictionary” thay vì map.

&nbsp;&nbsp;Nó thường trông như thế này:
```yaml
mysql:
  host: localhost
  user: root
  password: something
preprocessing_queue:  # Line comments are available!
  - name: preprocessing.scale_and_center
    width: 32
    height: 32
  - preprocessing.dot_reduction
use_anonymous: true
```

### Các ký hiệu tương đương nhau

YAML có rất nhiều cách tương đương để viết nội dung:
```yaml
list_by_dash:
  - foo
  - bar
list_by_square_bracets: [foo, bar]
map_by_indentation:
  foo: bar
  bar: baz
map_by_curly_braces: {foo: bar, bar: baz}
string_no_quotes: Monty Python
string_double_quotes: "Monty Python"
string_single_quotes: 'Monty Python'
bool_english: yes
bool_english_no: no
bool_python: True
bool_json: true
```

Một số từ cần chú ý:
```yaml
language: no  # ISO 639-1 code for the Norwegian language
```

Như ví dụ trên thì từ `no` sẽ được hiểu là `false`. Bạn cần phải viết là "no" hoặc 'no'.


Nói chung, tôi khuyên bạn nên sử dụng `true` và `false` giống như JSON làm cho boolean, nhưng YAML hỗ trợ [11 cách để viết boolean](https://yaml.org/type/bool.html).
Nếu bạn muốn sử dụng dấu ngoặc kép cho chuỗi, tôi thì sử dụng " giống như JSON. Bạn vẫn cần nhớ "no", nhưng ít nhất tệp trông quen thuộc hơn với người mới bắt đầu YAML.

### Đối với chuỗi dài có nhiều từ

```yaml
disclaimer: >
    Lorem ipsum dolor sit amet, consectetur adipiscing elit.
    In nec urna pellentesque, imperdiet urna vitae, hendrerit
    odio. Donec porta aliquet laoreet. Sed viverra tempus fringilla.
```

Điều này tương đương với JSON(các dòng mới được thêm vào để dễ đọc):

```yaml
{"disclaimer": "Lorem ipsum dolor sit amet, consectetur adipiscing elit.
In nec urna pellentesque, imperdiet urna vitae, hendrerit odio.
Donec porta aliquet laoreet. Sed viverra tempus fringilla."}
```

### Đối với chuỗi có nhiều dòng

```yaml
mail_signature: |
      Martin Thoma
      Tel. +49 123 4567
```

Điều này tương đương với JSON:

```yaml
{"mail_signature": "Martin Thoma\nTel. +49 123 4567"}
``

Lưu ý cách bỏ qua khoảng trắng ở đầu. Dòng đầu tiên (“Martin Thoma”) xác định số khoảng trắng hàng đầu bị bỏ qua.

### Anchor
```yaml
email: &emailAddress "info@example.de"
id: *emailAddress
```
Điều này tương đương với JSON:
```yaml
{"email": "info@example.de", "id": "info@example.de"}
```
`&` xác định biến emailAddress với giá trị "info@example.de". Sau đó dấu `*` chỉ ra rằng nó sẽ lấy theo giá trị của biến đó.

Bạn có thể làm tương tự với mappings:
```yaml
foo: &default_settings
  db:
    host: localhost
    name: main_db
    port: 1337
  email:
    admin: admin@example.com
prod: *default_settings
dev: *default_settings
```
 ta sẽ nhận được:
 ```yaml
 { "dev": { "db": {"host":
                  "localhost",
                  "name": "main_db",
                  "port": 1337},
           "email": {"admin": "admin@example.com"}},
  "foo": { "db": {"host": "localhost",
                  "name": "main_db",
                  "port": 1337},
           "email": {"admin": "admin@example.com"}},
  "prod": { "db": {"host": "localhost",
                   "name": "main_db",
                   "port": 1337},
            "email": {"admin": "admin@example.com"}}}
 ```

Bây giờ bạn có thể muốn chèn mật khẩu vào cài đặt `dev` và `prod`. Bạn có thể làm điều đó bằng cách sử dụng `<<`:

```yaml
foo: &default_settings
  db:
    host: localhost
    name: main_db
    port: 1337
  email:
    admin: admin@example.com
prod:
  <<: *default_settings
  app:
    port: 80
dev: *default_settings
```

Điều này tương đương với JSON:


```yaml
{ "foo": { "db": {"host": "localhost",
                  "name": "main_db",
                  "port": 1337},
           "email": {"admin": "admin@example.com"}},
  "prod": { "app": {"port": 80},
            "db": {"host": "localhost",
                   "name": "main_db",
                   "port": 1337},
            "email": {"admin": "admin@example.com"}},
  "dev": { "db": {"host": "localhost",
                  "name": "main_db",
                  "port": 1337},
           "email": {"admin": "admin@example.com"}},}
```

### Type Casting

2 ký tự `!!` có ý nghĩa đặc biệt trong YAML. Nó được gọi là "secondary tag handle" và viết tắt cho `!tag:yaml.org,2002` ([source](https://yaml.org/spec/1.2/spec.html#id2782457)).


Bạn có thể thực hiện các chuyển đổi đơn giản như sau:
```yaml
price: !!float 42
id: !!str 42
```
Hoặc một cái phức tạp hơn, ví dụ: ánh xạ trực tiếp đến các loại dữ liệu của Python mặc định không được chỉ định trong YAML:

```yaml
tuple_example: !!python/tuple
  - 1337
  - 42
set_example: !!set {1337, 42}
date_example: !!timestamp 2020-12-31
```

Bạn có thể đọc nó với:
```python
import yaml
import pprint
with open("example.yaml") as fp:
    data = fp.read()
pp = pprint.PrettyPrinter(indent=4)
pased = yaml.unsafe_load(data)
pp.pprint(pased)
```

Và bạn nhận được:

```yaml
{   'date_example': datetime.date(2020, 12, 31),
    'set_example': {1337, 42},
    'tuple_example': (1337, 42)}
```

Ví dụ này sử dụng thẻ dành riêng cho Python `!!python/tuple` và một số thẻ YAML tiêu chuẩn. PyYaml có ví dụ sau:
```
## Standard YAML tags
YAML               Python 3
!!null             None
!!bool             bool
!!int              int
!!float            float
!!binary           bytes
!!timestamp        datetime.datetime
!!omap, !!pairs    list of pairs
!!set              set
!!str              str
!!seq              list
!!map              dict
## Python-specific tags
YAML               Python 3
!!python/none      None
!!python/bool      bool
!!python/bytes     bytes
!!python/str       str
!!python/unicode   str
!!python/int       int
!!python/long      int
!!python/float     float
!!python/complex   complex
!!python/list      list
!!python/tuple     tuple
!!python/dict      dict
## Complex Python tags
!!python/name:module.name         module.name
!!python/module:package.module    package.module
!!python/object:module.cls        module.cls instance
!!python/object/new:module.cls    module.cls instance
!!python/object/apply:module.f    value of f(...)
```


Xin lưu ý rằng việc tải các thẻ không chuẩn là không an toàn. Có thể thực thi mã tùy ý với `!!python/object/apply:module.f`. Trong PyYaml, bạn cần `yaml.unsafe_load`
để sử dụng nó. Do đó bạn không nên sử dụng nó.

### Multiple Documents trong một YAML
Ta có thể sử dụng cú pháp sau:
```yaml
foo: bar
---
fizz: buzz
```

Trong Python, bạn có thể tải nó như với PyYAML:

```yaml
import yaml
with open("example.yaml") as fp:
    data = fp.read()
parsed = yaml.safe_load_all(data)  # parsed is a generator
```

Nếu bạn chuyển chúng sang dạng list và in nó, bạn sẽ nhận được:
```yaml
[{'foo': 'bar'}, {'fizz': 'buzz'}]
```

Xin lưu ý rằng đây không phải là một ký hiệu thay thế để viết danh sách. Đó là các tài liệu khác nhau.

## Tài liệu tham khảo
https://levelup.gitconnected.com/6-yaml-features-most-programmers-dont-know-164762343af3
