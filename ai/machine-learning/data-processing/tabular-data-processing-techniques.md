# Kỹ thuật xử lý dữ liệu dạng bảng (Tabular data processing techniques)
## 1. Phân tích Khám phá Dữ liệu (EDA - Exploratory Data Analysis)
### 1.1 Kích thước dữ liệu
Trước tiên, bạn cần hình dung được dữ liệu có khoảng bao nhiêu mẫu và có bao nhiêu trường dữ liệu. Nếu dữ liệu quá ít, khả năng cao là bạn không thể dùng Deep Learning để giải quyết mà cần dùng các phương pháp khác. Biết về kích thước dữ liệu cũng giúp bạn xác định kích thước tập huấn luyện (training data) và tập kiểm định (validation data) cũng như chuẩn bị bộ nhớ phù hợp.
### 1.2 Kiểu dữ liệu của mỗi trường
Các mô hình Machine Learning cho dữ liệu dạng bảng khá nhạy cảm với kiểu dữ liệu. Nhìn chung, các mô hình Machine Learning đều nhận dữ liệu đã qua xử lý ở dạng số. Một trong những yêu cầu của một mô hình tốt là tính ổn định với những thay đổi nhỏ ở đầu vào. Điều này tức là nếu đầu vào mô hình là những giá trị gần nhau thì đầu ra cũng được kỳ vọng là có giá trị gần nhau. Nếu một dữ liệu dạng hạng mục được mã hóa về số, chẳng hạn mã số người dùng, mà bạn nghĩ nó là dạng số thì mô hình sẽ học được rằng những người dùng có mã số gần nhau sẽ có những đặc trưng gần giống nhau. Điều này khả năng rất cao không đúng, nhất là khi mã số được đánh một cách ngẫu nhiên.
### 1.3 Phân phối xác suất của từng trường
Ta cần nắm được phân phối xác suất của từng trường dữ liệu để lên kế hoạch làm sạch dữ liệu và tạo các đặc trưng liên quan tới dữ liệu đó. Với mỗi cột dữ liệu, những trường hợp sau đây ta cần lưu tâm:

- Mọi giá trị trong cột bằng nhau: Ví dụ, trong dữ liệu có một cột là “Năm” và mọi giá trị đều bằng 2020. Như vậy cột này không mang lại ý nghĩa dự đoán. Ta có thể xóa cột này khi làm sạch dữ liệu.

- Có quá nhiều giá trị bị khuyết: Nếu thấy ý nghĩa cột này không quan trọng, ta có thể xóa. Nếu nó quan trọng, bạn cần có những chiến lược phù hợp (Xem Xử lý dữ liệu bị khuyết).

- Xuất hiện giá trị không hợp lệ: Nếu trường dữ liệu “Tuổi” mang những giá trị là số âm hoặc lớn hơn 200, khả năng cao chúng là những giá trị không hợp lệ. Với những giá trị không hợp lệ, ta có thể gán lại nó về giá trị hợp lệ gần nhất hoặc coi như dữ liệu bị khuyết.

- Xuất hiện giá trị ngoại lệ: Giả sử trường thông tin thu nhập hàng tháng chứa hầu hết giá trị trong khoảng từ 1-100 triệu đồng nhưng có một vài trường hợp ngoại lệ kiếm được tới 10 tỉ. Nếu giữ nguyên giá trị 10 tỉ đó, mô hình dường như được huấn luyện gượng ép, nó phải căng sức dự đoán những giá trị ngoại lệ đó khiến chất lượng đối với những giá trị phổ biến bị ảnh hưởng. Ngoài ra, nếu phải làm bước chuẩn hóa dữ liệu về đoạn [0,1] (một kỹ thuật rất phố biến khi xử lý dữ liệu) thì hầu hết các giá trị nằm trong khoảng [0,0.01]. Những giá trị rất nhỏ và gần nhau này có thể dẫn đến việc mô hình không phân biệt được sự khác nhau giữa các mức thu nhập khác nhau. Đó là ví dụ với dữ liệu dạng số. Với dữ liệu dạng hạng mục có nhiều hạng mục khác nhau, nếu một số hạng mục chiếm tới 99% tổng số mẫu trong khi tổng số mẫu của nhiều hạng mục khác lại chỉ có 1%. Ta cần có những cách xử lý đặc biệt với dữ liệu loại này.

### 1.4 Mối tương quan giữa các trường dữ liệu
Khi làm EDA, ta cũng cần tính toán độ tương quan giữa các trường dữ liệu, đặc biệt là giữa nhãn dự đoán và các trường còn lại. Nếu hệ số tương quan giữa một cột và cột nhãn bằng không, khả năng cao cột đó không mang lại nhiều giá trị dự đoán. Bạn có thể giành sự ưu tiên cho những cột có độ tương quan lớn hơn. Ngược lại, nếu hệ số tương quan giữa một cột và cột nhãn quá cao, có hai khả năng xảy ra:

- Cột đó có khả năng mang lại kết quả dự đoán tốt. Khi đó ta cần tập trung làm sạch và xây dựng các đặc trưng liên quan đến cột này trước.

- Dữ liệu có thể bị rò rỉ (data leakage). Giả sử bạn cần dự đoán tuổi của người dùng và nhận ra một cột có hệ số tương quan bằng 1. Nếu đó là cột “Năm sinh” thì rõ ràng bạn chẳng cần làm mô hình Machine Learning mà chỉ cần một phép trừ. Tuy nhiên, bài toán có thể là dự đoán tuổi khi trường “Năm sinh” bị khuyết nhưng biết nhiều thông tin khác. Rõ ràng ta không thể dùng trường “Năm sinh” trong trường hợp này mà cần loại bỏ nó đi.

Ngoài ra, nếu hai cột không phải cột nhãn mà có độ tương quan cao, ta cũng nên kiểm tra ý nghĩa của chúng xem có thể bỏ qua một cột hay không.

## 2. Làm sạch dữ liệu
### 2.1 Handling Outliers - Xử lý các giá trị ngoại lệ
Với dạng số, dữ liệu ngoại lệ có thể là một giá trị phi thực tế như số tuổi âm, hoặc một giá trị khác xa với phần còn lại của các giá trị trong trường đó. Với dạng hạng mục, dữ liệu ngoại lệ có thể là một giá trị phi thực tế như một hạng mục nằm ngoài những khả năng có thể xảy ra như một địa danh không có trên bản đồ. Các giá trị có tần xuất xảy ra vô cùng thấp trong một cột dữ liệu cũng có khả năng 2 là một giá trị ngoại lệ.
#### 2.1.1 Dữ liệu dạng số (Numeric data)
Có hai nhóm các giá trị ngoại lệ:

- Các giá trị không nằm trong miền xác định của dữ liệu. Ví dụ, tuổi, thu nhập hay khoảng cách không thể là số âm.

- Các giá trị có khả năng xảy ra nhưng xác suất rất thấp. Ví dụ, 120 tuổi, thu nhập 1 triệu đô la/tháng. Những giá trị này có khả năng xảy ra nhưng thực sự hiếm có.

Nhìn chung, chúng ta luôn có thể xóa bỏ cột hoặc hàng có dữ liệu ngoại lệ. Nếu xóa bỏ cột, ta có thể lãng phí rất nhiều các giá trị không phải ngoại lệ ở các hàng khác. Nếu xóa bỏ hàng, chúng ta cần lưu ý tới cách xử lý với dữ liệu mới. Tức là nếu một điểm dữ liệu mới cũng có giá trị ngoại lệ thì sao? Ta không thể bỏ không dự đoán điểm đó mà phải có cách biến đổi dữ liệu ngoại lệ này về những giá trị hợp lý hơn.

Với dữ liệu thuộc nhóm thứ nhất, ta có thể thay nó bằng nan và coi như một giá trị bị khuyết. Đôi khi những giá trị bị khuyết được mã hóa bằng một giá trị đặc biệt không nằm trong miền giá trị khả dĩ của dữ liệu. Khi coi chúng là giá trị bị khuyết, ta có thể xử lý tiếp như trong ref{sec_missing_data}.

Với dữ liệu thuộc nhóm thứ hai, người ta thường dùng phương pháp chặn trên hoặc chặn dưới (clipping hay capping). Tức là khi một giá trị quá lớn hoặc quá nhỏ, ta đưa nó về giá trị lớn nhất/nhỏ nhất được coi là những điểm bình thường. Ví dụ với một giá trị của tuổi là 120, ta có thể đưa nó về 70 và giả sử như điểm dữ liệu này có những đặc tính chung của “người cao tuổi”. Một điểm đáng lưu ý là việc chọn giá trị lớn nhất/nhỏ nhất cũng tùy thuộc vào dữ liệu. Nếu dữ liệu chỉ toàn bao gồm người cao tuổi tử 65 trở lên thì rõ ràng chặn trên bởi 70 là không hợp lý vì 70 vẫn là quá trẻ trong bộ dữ liệu này.

Vậy làm thế nào để chọn những giá trị lớn nhất, nhỏ nhất đó?
##### Box Plot
Cách phổ biến nhất là sử dụng Box plot. Box plot vừa giúp xác định xem dữ liệu có điểm ngoại lệ không, vừa giúp tìm ra ngưỡng lớn nhất và nhỏ nhất để làm điểm cắt.
##### Z Score
Phương pháp z score khá nhạy cảm với nhiễu lớn và không ổn định bằng phương pháp box plot.
#### 2.1.2 Dữ liệu hạng mục (Category data)
Với dữ liệu hạng mục, giá trị ngoại lệ có thể xảy ra ở một trong các trường hợp sau:

- Do sai khác trong cách nhập dữ liệu. Ví dụ, một phần dữ liệu thu được ở dạng viết hoa, một phần nhỏ khác lại ở dạng viết thường, như “VIỆT NAM” và “việt nam”. Một ví dụ khác là việc một hạng mục có nhiều tên khác nhau, chẳng hạn “Thành phố” và “Tp”. Trong trường hợp này, ta cần chuẩn hóa các giá trị về cùng một dạng để loại bỏ các điểm ngoại lệ.

- Do lỗi chính tả khiến một vài mẫu có giá trị khác hẳn với phần còn lại. Để xử lý dữ liệu sai lỗi chính tả, ta có thể vẽ histogram thể hiện tần suất của từng giá trị trong toàn bộ dữ liệu. Thông thường, những lỗi chính tả nằm ở những hạng mục có tần suất thấp. Những lỗi này cần phải được sửa trước khi đi vào bước tiếp theo.

- Một vài hạng mục xuất hiện quá ít trong dữ liệu. Các giá trị này có thể cần xử lý hoặc không, câu trả lời có thể đạt được thông qua thí nghiệm. Những hạng mục có tần suất thấp dễ khiến mô hình bị overfitting. Tuy nhiên, có những trường hợp mà những giá trị này có quan hệ chặt chẽ tới cột nhãn ta không nên bỏ đi. Nếu cần phải xử lý, một cách thông dụng là nhóm các hạng mục có tần suất thấp thành một hạng mục mới, có thể đặt tên là “rare” (khan hiếm).

- Hạng mục không xuất hiện trong tập huấn luyện. Rất nhiều trường hợp mà một giá trị ở bước “Serving” chưa từng xuất hiện trong cơ sở dữ liệu cũ. Điều này thường xuyên xảy ra với các hệ thống gợi ý khi người dùng và sản phẩm thay đổi liên tục. Với trường hợp này, cách phổ biến là tạo thêm một hạng mục mới gọi là “unknown” (chưa biết) khi xây dựng dữ liệu huấn luyện. Bất cứ khi nào có một giá trị mới, ta có thể xếp nó vào hạng mục này.
### 2.2 Xử lý dữ liệu bị khuyết
Với dữ liệu bảng, việc các trường thông tin bị khuyết (giá trị NaN) là thường xuyên xảy ra.

- Phương án loại bỏ dữ liệu:
  - Một cách đơn giản là bỏ cả cột thông tin có dữ liệu bị khuyết ra khỏi quá trình xây dựng mô hình. Cách làm này đơn giản nhưng có những hạn chế nhất định. Nếu có quá nhiều cột có dữ liệu bị khuyết và ta đều bỏ các cột này đi thì sẽ không còn thông tin gì cho việc xây dựng mô hình.

  - Cách thứ hai là bỏ đi hàng có giá trị bị khuyết đó khỏi tập huấn luyện. Việc này cũng có hạn chế tương tự như trên nên mỗi hàng mất một chút dữ liệu. Hạn chế thứ hai là khi gặp một điểm dữ liệu mới mà mô hình cần dự đoán, ta không thể đơn giản bỏ điểm dữ liệu đó đi mà vẫn phải dự đoán ra một giá trị nào đó.

  Có một quy tắc chung là nếu một cột có quá nhiều dữ liệu bị khuyết thì ta có thể bỏ cột đó. Việc tìm ngưỡng thế nào là quá nhiều tùy thuộc vào tính chất của dữ liệu và kinh nghiệm của các kỹ sư. Nếu dữ liệu có quá ít thông tin và lại bỏ bớt đi có thể làm giảm chất lượng mô hình.

- Phương án giữ lại dữ liệu:

  - Thứ nhất là tạo một cột mới is_nan mang thông tin dữ liệu có bị khuyết hay không. Đôi khi việc khuyết thông tin cũng chính là một thông tin quý giá. Một người không khai báo số điện thoại có thể vì họ không có điện thoại, một người giấu địa chỉ IP của máy chứng tỏ họ đề cao quyền riêng tư và có kỹ năng nhất định về máy tính. Như vậy việc thiếu thông tin cũng chính là một thông tin có thể khai thác.

  - Cách thứ hai giúp ta có thể giải quyết vấn đề dữ liệu bị khuyết là “điền” (impute) các giá trị bị khuyết một giá trị nào đó (sử dụng giá trị trung bình, trung vị hoặc mode - mean, median, mode) rồi dùng giá trị đó để xây dựng mô hình.

#### 2.2.1 Dữ liệu dạng số (Numeric data)
Với dữ liệu dạng số, hai cách phổ biến và đơn giản nhất là điền các giá trị bị khuyết bằng trung bình cộng hoặc trung vị của các giá trị không bị khuyết. Đây là các lựa chọn an toàn vì trung bình cộng hoặc trung vị là các giá trị có khả năng cao xảy ra. Một điểm đáng lưu ý là việc lấy trung bình hay trung vị này nên được cân nhắc dựa trên dữ liệu trước hoặc sau khi xử lý các điểm ngoại lệ.
#### 2.2.2 Dữ liệu hạng mục (Category data)
Với dữ liệu hạng mục, vì ta không tính được giá trị trung bình nên cách thường dùng là điền vào giá trị xuất hiện nhiều nhất (strategy='mode') hoặc coi chính việc bị khuyết là một giá trị đặc biệt (strategy='constant') với giá trị đặc biệt được truyền qua biến fill_value


## 3. Đặc trưng hạng mục (Category data)

### 3.1 Mã hoá One-hot (One-hot encoding)

Cách truyền thống nhất để đưa dữ liệu hạng mục về dạng số là mã hóa one-hot. Trong cách mã hóa này, một “từ điển” cần được xây dựng chứa tất cả các giá trị khả dĩ của từng dữ liệu hạng mục. Sau đó mỗi giá trị hạng mục sẽ được mã hóa bằng một vector nhị phân với toàn bộ các phần tử bằng 0 trừ một phần tử bằng 1 tương ứng với vị trí của giá trị hạng mục đó trong từ điển.

Ví dụ, nếu ta có dữ liệu một cột là "Sài Gòn", "Huế", "Hà Nội" thì ta thực hiện các bước sau:

  1. Xây dựng từ điển. Trong trường hợp này ta có thể xây dựng từ điển là ["Hà Nội", "Huế", "Sài Gòn"]

  2. Sau khi xây dựng được từ điển ta cần lưu lại chỉ số của từng hạng mục trong từ điển. Với từ điển như trên, chỉ số tương ứng là "Hà Nội": 0, "Huế": 1, "Sài Gòn": 2.

  3. Cuối cùng, ta mã hóa các giá trị ban đầu như sau:

Với từ điền thứ nhất:

|Giá trị| Mã One-hot|
|-------|-----------|
|"Sài Gòn"|[0, 0, 1]|
|"Huế"|[0, 1, 0]|
|"Hà Nội"|[1, 0, 0]

Vì mỗi giá trị hạng mục được mã hóa bằng một vector với chỉ một phần tử bằng 1 tại vị trí tương ứng của nó trong từ điển nên vector này được gọi là “one-hot vector”. Số chiều của vector này đúng bằng số từ trong từ điển. Diễn giải theo một cách khác, mỗi giá trị nhị phân trong vector này thể hiện việc giá trị hạng mục đang xét “có phải là” giá trị tương ứng trong từ điển không. Với các giá trị mới không nằm trong từ điển (out-of-vocabolary hay OOV), ta có thể mã hóa chúng thành [0, 0, 0] theo nghĩa chúng không phải là bất cứ một giá trị nào trong từ điển.

Có một cách khác phổ biến để mã hóa các giá trị không có trong từ điển là thêm từ "unknown" vào trong từ điển và tất cả những giá trị mới được xếp vào mục "unknown" này. Cần lưu ý khi "unknown" cũng là một giá trị khả dĩ trong tập dữ liệu. Việc mã hóa các giá trị chưa biết bằng cùng một vector có thể gây cho mô hình nhầm lẫn rằng đây là hai giá trị giống nhau. Nếu bằng một cách nào đó, bạn biết các giá trị này xuất hiện nhiều trong tương lai, bạn nên đưa chúng vào trong từ điển một cách cụ thể để có cách mã hóa riêng, tránh trùng lặp với các giá trị khác. Nếu các giá trị này hiếm khi xảy ra, ta có thể cho chung vào một mã và coi như chúng có tính chất giống nhau là “hiếm”. Cố gắng mã hóa cho từng giá trị hiếm sẽ dẫn đến tình trạng phải dùng nhiều bộ nhớ và mô hình cũng phức tạp hơn để cố gắng học những trường hợp cá biệt, khi đó overfitting dễ xảy ra.

#### Ví dụ với sklearn
Dưới đây là một ví dụ về việc mã hóa one-hot sử dụng sklearn.preprocessing.OneHotEncoder. Trước tiên,
``` python
import pandas as pd
from sklearn.preprocessing import OneHotEncoder

df_train = pd.DataFrame(
    data={"location": ["Hà Nội", "Huế", "Sài Gòn"], "population (M)": [7, 9, 0.5]}
)
df_train
```

||location	|population (M)|
|-|---------|--------------|
|0|Hà Nội|7.0|
|1|Huế|9.0|
|2|Sài Gòn|0.5|

Tiếp theo, ta áp dụng mã hóa one-hot lên cột location:
``` python
onehot = OneHotEncoder()

onehot_encoded_location = onehot.fit_transform(df_train[["location"]])
print(type(onehot_encoded_location))
print(onehot_encoded_location)
# --------- Output
<class 'scipy.sparse.csr.csr_matrix'>
  (0, 1)	1.0
  (1, 0)	1.0
  (2, 2)	1.0
```
Có một vài điểm cần lưu ý ở đây. Thứ nhất, kết quả trả về onehot_encoded_location mặc định được lưu ở kiểu scipy.sparse.csr.csr_matrix là một kiểu đặc biệt để lưu các mảng hai chiều có phần lớn các phần tử bằng 0. Cách lưu này rất thuận lợi về mặt bộ nhớ trong trường hợp này vì mỗi vector chỉ có đúng một phần tử khác 0. Nếu kích thước từ điển tăng lên tới hàng triệu và ta lưu ma trận ở dạng thông thường thì sẽ rất lãng phí tài nguyên khi phải lưu rất nhiều giá trị 0 không mang nhiều thông tin.

Khi in onehot_encoded_location ra ta sẽ thấy hay cột. Cột thứ nhất là tọa độ của các điểm khác 0, cột thứ hai là giá trị của phần tử ở tọa độ đó – luôn luôn bằng 1 trong trường hợp này.

Để trả về kết quả ở dạng ma trận thông thường, ta có thể thêm sparse=False khi khởi tạo:
``` python
onehot = OneHotEncoder(sparse=False)

onehot_encoded_location = onehot.fit_transform(df_train[["location"]])
print(onehot_encoded_location)
# --------- Output
[[0. 1. 0.]
 [1. 0. 0.]
 [0. 0. 1.]]
```
Như vậy, thứ tự các hạng mục trong từ điển đã được đảo đi:
``` python
onehot.categories_
# --------- Output
[array(['Huế', 'Hà Nội', 'Sài Gòn'], dtype=object)]
```
Chúng ta cần lưu lại thứ tự này để có sự nhất quán khi mã hóa các dữ liệu về sau.

Với các giá trị không có trong từ điển, sklearn cung cấp hai cách xử lý thông qua biến handle_unknown (xem thêm tại liệu chính thức để biết thêm chi tiết). Biến này có thể nhận một trong hai giá trị 'error' (mặc định) hoặc 'ignore'. Với 'error', chương trình sẽ dừng chạy và báo lỗi khi gặp một giá trị không nằm trong từ điển. Với 'ignore', bộ mã hóa này sẽ biến đổi các giá trị lạ về vector toàn 0. Rất tiếc, bộ mã hóa này không hỗ trợ trường hợp gộp các giá trị mới vào một hạng mục riêng. Việc sử dụng 'error' và 'ignore' tùy thuộc vào ngữ cảnh. Nếu bạn biết chắc chắn toàn bộ các giá trị khả dĩ của dữ liệu hạng mục đó thì nên dùng 'error' để bắt được các trường hợp đầu vào bị lỗi. Ngược lại, bạn nên dùng 'ignore'; tuy nhiên, cần lưu ý với các trường hợp viết sai chính tả!

#### Kết luận
1. Ngoại trừ trường hợp một giá trị chưa biết được mã hóa thành vector 0, mỗi vector đều có khoảng cách Euclid tới một vector khác bằng $\sqrt{2}`. Việc này không thể hiện được việc các hạng mục có nét tương đồng với nhau.

2. Mã hóa one-hot là một cách biến đổi nhanh chóng từ dữ liệu dạng hạng mục sang dạng số. Với cách mã hóa này, ta có thể xây dựng nhanh chóng các mô hình đơn giản như hồi quy tuyến tính hay SVM, các mô hình này bắt buộc giá trị đầu vào là ở dạng số. Với các mô hình dạng cây quyết định (Random Forest, LightGBM, XGBoost, v.v.) – rất phổ biến với dữ liệu dạng bảng, ta không cần đưa về dạng onehot mà chỉ cần đưa về dạng số thứ tự trong từ điển và báo với mô hình rằng đó là đặc trưng hạng mục, các mô hình sẽ có cách xử lý phù hợp (Xem thêm sklearn.preprocessing.LabelEncoder).

3. Với mã hóa onehot, ta cần lưu lại từ điển để tính toán vector cho các giá trị khác trong tương lai. Việc này có hạn chế lớn là cần thêm bộ nhớ và cần biết chính xác số lượng giá trị khả dĩ của dữ liệu. Một kỹ thuật có chức năng gần tương tự được sử dụng nhiều hơn là Hashing sẽ được trình bày ở mục tiếp theo.

4. Mã hóa onehot đặc biệt thiếu hiệu quả khi từ điển lớn lên, số chiều của dữ liệu đầu vào lớn sẽ khiến các mô hình machine learning khó học hơn với những đầu vào có số chiều thấp. Một cách cực kỳ hiệu quả và phổ biến khác là xây dựng các embedding biến từng hạng mục thành một vector dày (dense vector) có số chiều thấp hơn thay vì ở dạng onehot là vector thưa (sparse vector). Các embedding vector cũng sẽ thể hiện được tự tương đồng giữa các hạng mục tốt hơn so với onehot. Chúng ta sẽ thảo luận tới kỹ thuật này trong mục Embedding.

### 3.2 Hashing
One-hot encoding có một hạn chế lớn là cần biết trước từ điển và kích thước của nó. Từ điển này cũng cần được lưu lại để mã hóa các hạng mục mới trong tương lai. Thử tưởng tượng một cửa hàng thương mại điện tử có 1000 mặt hàng trong hôm nay nhưng trong một tháng tới, cửa hàng đó có thêm 1000 mặt hàng mới. Vậy các mặt hàng mới đó nên được mã hóa như thế nào? Rõ ràng việc mã hóa chúng bằng vector 0 hay bằng one-hot vector tương ứng với "unknown" sẽ làm giảm chất lượng mô hình vì không có sự phân biệt rạch ròi giữa 1000 mặt hàng mới. Lúc này, để có thể tiếp tục sử dụng mã hóa one-hot, ta cần cập nhật từ điển và mã hóa lại toàn bộ các giá trị hạng mục. Điều này đồng nghĩa với việc đầu vào của mô hình sẽ thay đổi và khả năng cao ta cũng cần thay đổi kiến trúc của mô hình để thích ứng với sự thay đổi kích thước đầu vào.

Một kỹ thuật được sử dụng rất nhiều để giải quyết vấn đề này là hashing (hàm băm). Hashing là một phép biến đổi giá trị đầu vào bất kỳ thành một số nguyên. Một hàm hash tốt là hàm có đặc điểm biến những giá trị đầu vào khác nhau thành các điểm phân bố đều trong khoảng giá trị khả dĩ (các số nguyên 32 bit hoặc nhiều hơn tùy thuộc vào hàm hash). Một đặc điểm khác là các giá trị đầu vào khác nhau sẽ được biến thành các số nguyên có khả năng cao khác nhau, đặc biệt khi dùng số lượng bit lớn.

Để sử dụng hashing như một cách biến đổi các giá trị hạng mục về một số tự nhiên được sử dụng trong các mô hình machine learning, ta có thể thực hiện các bước sau:

  1. Biến đổi các giá trị hạng mục về dạng string (một số hàm hash chỉ chấp nhận đầu vào là dạng string).

  2. Lựa chọn một hàm hash “tất định”, tức một hàm luôn trả về một số cố định ở tất cả các lần chạy nếu đầu vào không đổi. Việc này rất quan trọng vì nếu với một giá trị mà hàm hash trả về đầu ra khác nhau thì mô hình machine learning không thể biết được các đầu vào là một. Lưu ý rằng một số hàm hash có khả năng trả về các giá trị khác nhau, có thể vì lý do bảo mật, ta cần tránh sử dụng các hàm hash này.

  3. Ước lượng số lượng các phần tử khác nhau của dữ liệu hạng mục rồi chọn một số tự nhiên Klàm mod. Lấy phần dư của kết quả ở bước hai khi chia cho số K này làm chỉ số cho hạng mục tương ứng.
#### Ví dụ
Xét ví dụ về dữ liệu các sản phẩm trong bộ dữ liệu Predict Future Sales.
``` python
import pandas as pd

sales_path = "https://media.githubusercontent.com/media/tiepvupsu/tabml_data/master/sales/"
df_items = pd.read_csv(sales_path + "items.csv")
print(f"Number of items: {len(df_items)}")
print(f"Number of category: {len(df_items['item_category_id'].unique())}")
df_items.head(5)
# --------- Output
Number of items: 22170
Number of category: 84
```
||item_name|	item_id|	item_category_id|
|-|---|---|---|
|0	|! ВО ВЛАСТИ НАВАЖДЕНИЯ (ПЛАСТ.) D|	0|	40|
|1	|!ABBYY FineReader 12 Professional Edition Full...|	1|	76|
|2	|***В ЛУЧАХ СЛАВЫ (UNV) D	|2	|40|
|3	|***ГОЛУБАЯ ВОЛНА (Univ) D	|3	|40|
|4	|***КОРОБКА (СТЕКЛО) D	|4	|40|

Như vậy, có 22170 sản phẩm khác nhau được chia vào 84 hạng mục khác nhau. Các hạng mục này có thể được trực tiếp sử dụng để xây dựng one-hot vector. Ta cũng có thể thấy rằng sẽ có nhiều sản phẩm được chia vào cùng một hạng mục. Nếu không có đặc trưng khác để phân biệt các sản phẩm, ta sẽ xây dựng được một mô hình mà mọi sản phẩm trong cùng một hạng mục có cùng một tính chất. Để có thể tách biệt được các sản phẩm, ta có thể xử lý thêm thông tin về tên sản phẩm ở cột thứ nhất. Việc này sẽ tương đối khó khăn vì không phải kỹ sư nào cũng biết tiếng Nga. Một cách khác là sử dụng item_id làm đặc trưng hạng mục và xây dựng one-hot vector cho cột này với 22170 phần tử. Đây là số lượng phần tử tương đối lớn, ngoài ra trong dữ liệu huấn luyện (file sales_train.csv), rất nhiều item_id chỉ xuất hiện một lần. Nếu xây dựng one-hot với 22170 phần tử, khả năng cao mô hình sẽ bị overfitting khi có quá nhiều hạng mục có ít dữ liệu.

Hashing là một kỹ thuật khả dĩ có thể áp dụng lên item_name. Dưới đây là một triển khai đơn giản của kỹ thuật hashing được viết theo sklearn API với số lượng hash bucket là 1000:
``` python
import hashlib
from typing import Tuple

from sklearn.base import BaseEstimator, TransformerMixin


def hash_modulo(val, mod):
    md5 = hashlib.md5()  # can be other deterministic hash functions
    md5.update(str(val).encode())
    return int(md5.hexdigest(), 16) % mod


class FeatureHasher(BaseEstimator, TransformerMixin):
    def __init__(self, num_buckets: int):
        self.num_buckets = num_buckets

    def fit(self, X: pd.Series):
        return self

    def transform(self, X: pd.Series):
        return X.apply(lambda x: hash_modulo(x, self.num_buckets))


fh = FeatureHasher(num_buckets=1000)

df_items["hashed_item"] = fh.transform(df_items["item_name"])
df_items.head(5)
```
||item_name|	item_id|	item_category_id|hashed_item|
|-|---|---|---|---|
|0	|! ВО ВЛАСТИ НАВАЖДЕНИЯ (ПЛАСТ.) D|	0|	40|252|
|1	|!ABBYY FineReader 12 Professional Edition Full...|	1|	76|812|
|2	|***В ЛУЧАХ СЛАВЫ (UNV) D	|2	|40|198|
|3	|***ГОЛУБАЯ ВОЛНА (Univ) D	|3	|40|584|
|4	|***КОРОБКА (СТЕКЛО) D	|4	|40|210|
Lúc này, ta có thêm cột "hashed_item" với các giá trị không vượt quá 1000. Cột này cũng giúp phân biệt các sản phẩm trong hạng mục 40.
#### Kết luận
1. Hashing đặc biệt hữu ích khi ta không biết từ điển của một trường hạng mục. Nếu có giá trị OOV (ngoài từ điển), hashing vẫn tạo ra được một số tự nhiên và có thể đảm bảo được các giá trị chưa có trong từ điển nhận các mã khác nhau. Từ điển cùng với vị trí của từng từ trong đó không cần được lưu trong bộ nhớ. Kỹ thuật hash đặc biệt hữu ích với online learning khi ta không biết trước từ điển.

2. Với những hạng mục có lượng giá trị phân biệt lớn (high cardinality) nhưng có nhiều hạng mục có tần suất thấp, ta có thể chọn K
nhỏ để hạn chế việc tốn tài nguyên bộ nhớ và tránh overfitting. Điều này sẽ dẫn đến việc có nhiều giá trị đầu vào khác nhau cùng có một đầu ra (cùng hash bucket). Mặc dù vậy, các thuật toán machine learning sẽ vẫn có thể phân biệt được chúng dựa trên các đặc trưng khác.

3. Với những hạng mục mà việc xung đột hash (hash collision – các giá trị đầu vào khác nhau mà đầu ra của hàm hash cho cùng một giá trị) trở nên nghiêm trọng, ta có thể chọn số lượng bucket lớn để hạn chế việc xung đột. Việc chọn số lượng bucket như thế nào để xác suất xung đột thấp, bạn đọc có thể đọc thêm phân tích chi tiết trong Don’t be tricked by the Hashing Trick. Khi lượng bucket lớn, sẽ có nhiều bucket không có phần tử nào. Nếu sử dụng deep learning, các hệ số tương ứng với bucket này sẽ không được cập nhật mà sẽ là các giá trị ngẫu nhiên được xác định trong quá trình khởi tạo. Trong tương lai, nếu có một phần tử rơi vào bucket này, mô hình có thể sẽ đưa ra kết quả không tốt. Để giảm thiểu tình trạng này, ta có thể sử dụng các kỹ thuật điều chuẩn (regularization) như ℓ1 hoặc ℓ2 để đưa các hệ số tương ứng về gần với 0.
### 3.3 Crossing

Chúng ta thường quen với việc xử lý các đặc trưng một cách riêng biệt từ các trường dữ liệu tương ứng. Thực tế thì các trường dữ liệu có thể có mối quan hệ với nhau nhưng các mô hình machine learning đơn giản khó có thể hình dung ra được. Những mối quan hệ đó thường được khám phá ra bởi các nhà khoa học dữ liệu hoặc dựa trên kiến thức miền (domain knowledge).

Lấy một ví dụ nhỏ với bài toán dự đoán giá nhà California, ở đây kinh độ và vĩ độ là hai trường dữ liệu độc lập. Nếu tách biệt hai đặc trưng được tạo bởi hai trường dữ liệu này, mô hình có thể học được một tính chất rằng những vùng có cùng kinh độ hoặc vĩ độ sẽ có giá nhà gần với nhau. Điều này rõ ràng không đúng. Tuy nhiên, nếu có các thông tin về cả kinh độ và vĩ độ trong cùng một giá trị, mô hình sẽ học được những thông tin hữu ích hơn.

#### Kết luận
1. Về lý thuyết, các đặc trưng cấu thành đặc trưng chéo có thể là đặc trưng số. Tuy nhiên, việc này cần tránh vì có thể đặc trưng số có rất nhiều giá trị khác nhau và cả những giá trị chưa biết không xuất hiện trong tập huấn luyện. Nếu muốn sử dụng các đặc trưng số để tạo đặc trưng chéo, ta có thể chia đặc trưng số vào một lượng nhỏ các bin được định nghĩa trước. Việc này giúp đảm bảo đặc trưng chéo (dạng hạng mục) không có quá nhiều giá trị khác nhau.

2. Số lượng giá trị phân biệt của đặc trưng chéo có thể sẽ rất lớn nếu các đặc trưng thành phần cũng có số phần tử phân biệt lớn. Khi đó, đặc trực chéo sẽ được kết hợp với kỹ thuật Hashing để giảm số lượng phần tử riêng biệt. Cách làm này có thể gây ra xung đột hash nhưng vẫn mang lại hiệu quả cao trong nhiều trường hợp.

3. Việc chọn ra các tập đặc trưng nào để làm đặc trưng chéo có thể sẽ mất nhiều thời gian và công sức trong việc thí nghiệm. Năm 2017, một nhóm nghiên cứu ở Google đã đề xuất một phương pháp có tên “Deep and Cross Network” giúp tạo ra các đặc trưng chéo một cách tự động. Bạn đọc có thể tìm hiểu thêm trong mục Tài liệu tham khảo.


## 4. Đặc trưng dạng số (Numeric data)
Chưa có dữ liệu