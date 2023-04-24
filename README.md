# bof3
## Ý tưởng.
- Ta nại cùng tìm hiểu 1 kỹ thuật mới :( và mở màn bằng 1 bài ví dụ cơ bản ( má dl dí thì vl ).
![image](https://user-images.githubusercontent.com/130078745/233946098-ad09e129-7d37-4ba1-910b-fe6354eb6a05.png)
- Ta có thể thấy trong bài này ta có tới 2 hàm là hàm main và hàm win. Vào hàm win thì ta có thể thấy đc cách để lấy đc flag trong bài này.
![image](https://user-images.githubusercontent.com/130078745/233946553-27534fb0-059e-472e-9cce-2d8294f79cc8.png)
- Nhưng bởi vì là 2 hàm riêng biệt khi ta thực hiện 1 hàm con trong 1 hàm mẹ thì khi thực hiện xong và có kết quả nó sẽ phải quay về hàm mẹ, vì vậy việc của ta là làm cách nào để từ hàm con nó sẽ nhả sang 1 hàm khác theo ý muốn mà ko quay về hàm mẹ. Ta chỉ cần thay đổi địa chỉ thanh ghi **save rip**
## Thực Hành.
- Ta cùng vào terminal để debug , sử dụng lệnh **Disas main** tạo 1 breakpoint để debug nhanh và gọn hơn.
![image](https://user-images.githubusercontent.com/130078745/233951832-13a348a2-7f1a-4d56-a04d-e6bf67313dd7.png)
- Thử nhập dữ liệu xem biến nó tràn đc đến đâu nhóe :>> .
![image](https://user-images.githubusercontent.com/130078745/233952074-a67fc506-3a7d-4bfd-b9af-1e24efbd3391.png)
![image](https://user-images.githubusercontent.com/130078745/233952247-a3a33b3c-a202-4f08-ad29-0c083bbe05d2.png)
- Sau khi đã nhập dữ liệu và **ni** chạy đến lệnh return thì ta thấy được địa chỉ cũng như offset của biến **V5**.
- Ta in ra địa chỉ của hàm **Win** để có thể return giá trị đến đc hàm đó.
![image](https://user-images.githubusercontent.com/130078745/233953812-1da8f3d4-9143-4152-94c9-3059d5abe702.png)
- Sau khi ta đã nhập dữ liệu và return dữ liệu **V5** sẽ đc trả về hàm **libc_start_mai** , và nhiệm vụ của ta là thay đổi nó để dữ liệu đó đc trả về hàm **win**.
![image](https://user-images.githubusercontent.com/130078745/233955281-4169d018-cff1-4a35-bacc-d36ba2719d7a.png)
- Ta sẽ tiến hành viết tools để làm điều đó.
- Bước đầu là nhập vào 40byte để có thể đến offset của **V5** , sau đấy là sử dụng câu lệnh để thay đổi địa chỉ mà dữ liệu return.
- Để có thể lấy địa chỉ và thay đổi nó ta có 2 cách : 1 là sử dụng **p &win** trong terminal để lấy địa chỉ hàm win , 2 là sử dụng câu lênh trong python như sau.
![image](https://user-images.githubusercontent.com/130078745/233957009-33324752-f7fe-4563-b5cc-e5836726fab8.png)
- Chạy thử chương trình xem có đúng ko nhé.
![image](https://user-images.githubusercontent.com/130078745/233957472-6949d689-b38e-4658-8257-205597a19174.png)
- Khi chạy thử ta có thể thấy chương trình ta viết đã sai ở đâu đó , hãy sử dụng kỹ thuật debug động để xem lỗi như thế nào nhé.
- Tạo 1 input để chương trình dừng lại chỗ input.
![image](https://user-images.githubusercontent.com/130078745/233958415-69f63498-905a-4038-81fa-8523f35be91d.png)
- Chạy lại chương trình 1 lần nữa lấy **pid** vào terminal sử dụng lệnh **attach + pid(ta vừa lấy)**.
- Tạo 1 breakpoint ở chỗ return để xem đã thay đổi địa chỉ **save rip** hay chưa.
![image](https://user-images.githubusercontent.com/130078745/233960156-5f0600c0-86b3-4712-b53b-1a62bb21204d.png)
- Ở đây ta có thể thấy chúng ta đã thành công thây đổi địa chỉ **save rip** rồi.
![image](https://user-images.githubusercontent.com/130078745/233964344-29731807-dc42-4b91-9c2a-e885fd80fb96.png)
- Xem ở hàm system , vào check ở hàm system thì ta đã thấy 1 lỗi , lỗi ở đây rất cơ bản vì địa chỉ stack ở đây ko chia đc hết cho 16.
![image](https://user-images.githubusercontent.com/130078745/233961580-27502f15-8d2f-4f7b-84a4-70a747e3a851.png)
- Khi chia thử thì ta có thể thấy có số dư. ![image](https://user-images.githubusercontent.com/130078745/233961729-eb45a8d9-0415-4d2b-86b5-aefd046aead6.png)
- Ở trong hàm win này khi chạy đến hàm push nó sẽ nhảy về địa chỉ ở trên , và cũng ko nhất thiết là khi truy cập đầu hàm main ta có thể truy cập vào thẳng vào hàm mov để tránh trường hợp vào câu lệnh thay đổi địa chỉ vào 1 địa chỉ ko thể chia hết cho 16 và làm nó bị lỗi.
![image](https://user-images.githubusercontent.com/130078745/233964630-1572a8ed-8f7d-4860-b1a3-3b81340a1ab0.png)
![image](https://user-images.githubusercontent.com/130078745/233965142-7fd56e21-cb78-4fd1-8993-2170e31bcc2a.png)
- Vì vậy ta có thể nhảy đến trực tiếp tại dòng **win +5** để ko bị thay đổi địa chỉ và chia hết cho 16. 
![image](https://user-images.githubusercontent.com/130078745/233965899-ec4ba36e-0304-49ba-905c-3eaea69f9eb7.png)
- Sau khi sửa tools ta chạy thử và chương trình đã đúng.
![image](https://user-images.githubusercontent.com/130078745/233966341-382014fc-aa5c-4220-8289-5364cd07ad11.png)



