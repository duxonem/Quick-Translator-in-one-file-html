Quick Translator giới thiệu và đưa vào dùng Luật Nhân cho 1 số cấu trúc câu đặc biệt trong tiếng Trung do đặc điểm ngôn ngữ mà việc dùng Vietphrase thay chữ không làm được. Tuy nhiên cách tiếp cận hiện tại có mấy hạn chế (suy đoán):

- Chỉ dò và thay thế 1 cụm từ {0}, chưa thực hiện được trên nhiều cụm từ.
- Có vẻ là dùng các từ trong Pronouns.txt (hoặc thêm Names, Vietphrase) thay thế cho {0} trong file Luatnhan.txt. Như vậy sẽ tạo ra dữ  liệu rất lớn khiến cho chạy chậm. Giả sử file Luatnhan có 100 dòng, Pronoún có 100 dòng, Names có 1.000 dòng, như vậy khi dùng cách này sẽ tạo ra 100*100=10.000 dòng dữ liệu mới với chỉ Pronouns. Tạo ra (100+1.000)*100 = 110.000 dòng dữ liệu mới với Pronouns và Names.
- Có vẻ như dùng cách tách thành từng câu, thực hiện trên từng câu. Việc thực hiện như trên có thể sẽ chậm nếu convert trên cả bộ truyện. Một truyện hoàn chỉnh trung bình khoảng 5MB, mỗi chữ tiếng Trung chiếm 2bytes, như vậy 1 truyện có khoảng 2,5tr chữ. Trung bình 20 chữ 1 câu, ta có khoảng 125K câu. Giả sử file LuatNhan.txt có 100 quy tắc. như vậy chương trình sẽ chạy 125K*100=12M5 lần để convert các câu có cấu trúc trước khi thực hiện việc convert Vietphrase như khi không sử dụng.

Dưới đây là cách hàm sTrans(text) thực hiện, ở đây không sử dụng Pronounce do không cần thiết:

- Sử dụng nhiều cụm từ thay thế {0}, {1}... do đó có thể thay đổi vị trí, thứ tự của các cụm từ trên. Các cụm từ này có thể là bất kỳ không nhất thiết phải là Name, Vietphrase... trừ khi đứng đầu tiên hoặc sau cùng (Vd: {0}adcd{1}dffgkf{3} ).
- Đối với các cấu trúc mà {0} đứng đầu hoặc {n} sau cùng thì {0},{n} sẽ chỉ tìm trong Names, Vietphrase. Nếu không có thì trả lại câu cũ.
- Code này có hạn chế không dùng {0}, {1}... cho các số 1,2,3....hoặc các chữ Latin; chỉ dùng với chữ Hán. Có thể đưa vào sau nếu thấy ổn định.
- Các {0}.... không bao gồm dấu '.', ','... nói chung là các dấu chấm câu
- Code javascript dưới đây chưa kiểm kỹ, đặc biệt với trường hợp {n} đứng sau cùng, cho nên có thể cần chỉnh lại 1 chút về mặt con số.

  ```js
      const dictVP = { Han: ["殺死","毁了"], HanViet: {"殺死":"giết chết", "毁了":"hủy"} }; // dùng format này cho giống trong file index.html
      const dictNames = { Han: ['劳琳','劳拉','劳伦'], HanViet: {'劳琳':'Laurine','劳拉':'Laura','劳伦':'Lorene'} }; 
      const structuredPhrase = {
        "第{0}章": "chương {0}",
        "把{0}挡住": "ngăn cản {0}",
        "比{0}好多了": "tốt hơn {0} nhiều" };

  function structuredTrans(text) {
        const maxNamesLength = 7;  //giả sử cho nhanh, thực tế phải tìm length từ tiếng Trung dài nhất
        const maxVPLength = 15;
        const maxLength = Math.max(maxNamesLength, maxVPLength);
        let cText, vText;
        let searchReplace = {};
        let reg = /{\d}/g

        for (line in structuredPhrase) {
          cText = line;
          let searchElement = cText.match(reg);
          let strReg = cText.replace(reg, '(\\p{sc=Han}+)');
          let replacePhrase = [...text.matchAll(new RegExp(strReg, 'ug'))];

          if (replacePhrase && replacePhrase.length > 0)
            for (let i = 0; i < replacePhrase.length; i++) {
              vText = structuredPhrase[line];
              searchReplace['cText'] = replacePhrase[i][0];

              for (let j = 0; j < searchElement.length; j++) {
                let str = replacePhrase[i][j + 1];

                if (j == 0 && searchReplace['cText'].indexOf(str) == 0) { //dạng {0}ksjdhf......

                  for (k = 0; k < Math.min(maxLength, str.length); k++) {
                    let first = dictNames.HanViet[str.slice(k)] || dictVP.HanViet[str.slice(k)];
                    if (first != undefined) {
                      searchReplace[searchElement[j]] = first;
                      break;  //for k==0
                    }
                  } //End for

                  if (searchReplace[searchElement[i]] == undefined) {
                    vText = replacePhrase[i][0]; ///vText=cText
                    breakLoop = true;
                    break;
                  } // for j=0
                } //End if (j==0)

                if (j == searchElement.length - 1 && str == searchReplace['cText'].slice(-str.length)) { //dạng .....kdfhkhdf{n}
                  for (k = Math.min(maxLength, str.length); k > 0; k--) {
                    let last = dictNames.HanViet[str.slice(0, k)] || dictVP.HanViet[str.slice(0, k)];
                    if (last != undefined) {
                      searchReplace[searchElement[j]] = last;
                      break;  //for k=Math.min(maxlength, str.length)
                    }
                  } //End for
                  if (searchReplace[searchElement[j]] == undefined) {
                    vText = replacePhrase[i][0]; //cText
                    breakLoop = true;
                    break;  //break for j=0
                  }
                }
                searchReplace[searchElement[j]] = replacePhrase[i][j + 1];
                vText = vText.replace(searchElement[j], searchReplace[searchElement[j]]);
              } //End for (j=0)

              text = text.replace(searchReplace['cText'], vText);
              searchReplace = {};
            } //End for(i=0)  

        }//End for(line in structuredPhrase)
        return text;
      }
  ```
  
