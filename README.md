# Quick-Translator-in-one-file-html
### Giới thiệu

Chương trình này hoạt động giống Quick Translator nhưng chỉ có 1 file index.html chạy duy nhất và chạy trên trình duyệt web như Firefox, Chrome... Vì chạy trên trình duyệt nên nó độc lập với hệ điều hành, có thể chạy trên Mac, linux. Ngoài ra, trên Quick Translator khung Phiên Âm khá vô dụng nên được chuyển qua dùng giống CxDict là đọc và đánh dấu những chữ đã biết hiện thẳng chữ Trung.

### Cài đặt và cách dùng

Bạn có thể bỏ file index.html này lên 1 webserver để chạy trên mạng hoặc để trên máy tính dùng File explorer để mở trong trình duyệt. File rất nhỏ khoảng 65KB không cần cài thêm gì khác như .Net Framework đối với Quick Translator.

Tuy nhiên chương trình vẫn cần các file từ điển để chạy. Các file từ điển cần thiết là Names.txt dùng cho tên nhân vật, Vietphrase.txt để convert các từ Trung sang Việt, PhienAm.txt để chuyển về âm Hán Việt.

Ngoài ra có thể tải thêm các từ điển tra nghĩa để hiện khung tra nghĩa bên phải. Các từ điển được dùng trong chương trình này đều có dạng như các file Names,  Vietphrase. Cấu trúc : Tiengtrung=Tiengviet. Mỗi chữ/nhóm chữ là 1 dòng. Ví dụ:

 劳琳=Laurine\
 劳拉=Laura\
 劳伦=Lorene\
 凯西=Cathy\
 凯蒂=Katy

Để rút gọn, tối giản hóa, chương trình bỏ các nút bấm nhiều nhất có thể. Chỉ có 1 chỗ bấm ... để chọn menu Options. Khi chạy lần đầu nên vào đây để tải các từ điển cần thiết cho chương trình(PhienAm, Vietphrase, Names). Chạy ở toàn màn hình để có trải nghiệm tốt hơn.

Sau khi đã tải đủ các từ điển cần thiết, dán văn bản tiếng trung vào ô Trung bên trái, sau đó bấm qua bất kỳ chỗ nào khác thì chương trình sẽ convert và hiện lên ở khung Vietphrase bên phải. 

Để tra nghĩa từ nào thì bạn bấm vào, từ đó sẽ hiện đỏ và nghĩa sẽ hiện ở khu bên khung "Từ Điển" bên trái. Để hiện nghĩa thì bạn phải vào menu Options tải thêm các từ điển tra nghĩa. Bạn có thể đặt tên, sắp xếp thứ tự hiện trên khung tra từ điển ở đây.

Để thêm, sửa nghĩa Vietphrase, Names thao tác như trên Quick Translator.

Khung Phiên Âm sẽ chỉ hiện Hán Việt, chữ nào không biết có thể click vào để hiện nghĩa trên khung bên trái. Nếu từ nào đã đọc quen, muốn hiện thẳng chữ Trung thì double click vào chữ đó, để trở lại bấm dblClick lần nữa. 

Khung "Từ lặp" sẽ hiện những cụm từ lặp lại nhiều lần. Nó có khả năng cao là Name, Vietphrase. Bạn dễ dàng kiểm tra trong từ điển mình có chưa để thêm vào.

Để lưu/backup các file Names, Vietphrase chương trình có các menu export các file trên để lưu lại nơi khác

### Lưu ý

Với riêng Firefox, do các file từ điển lưu ở localStorage và indexedDB, nếu mở file trên nội bộ máy tính, khi chuyển qua nơi khác sẽ dẫn đến đổi đường dẫn. Điều này khiến trình duyệt hiểu giống như đổi tên miền khác dẫn đến các từ điển đã tải lên sẽ không có cho cho chương trình ở  đường dẫn mới. Vì vậy không nên chuyển vị trí nếu đã dùng quen.\
Các trình duyệt với nhân Chromium không bị tình trạng như trên.

Video hướng dẫn: https://streamable.com/w78fxd

Link thử https://qtwtest.000webhostapp.com/  
