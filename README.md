Giao thức xác thực dựa trên thuật toán ElGamal trên đường cong elliptic (Baccouri - 2023) được thực hiện như sau:
1. Quá trình sinh α và β
   Các bước của quá trình sinh như sau:
 1. Chọn ngẫu nhiên một số nguyên s trong khoảng [0, n- 1].
 2. Thực hiện phép nhân vô hướng: s * G = (x, y).
 3. Alice kiểm tra x và y theo điều kiện sau:
 ((x ∗ MaxASCIICode)+y) ∈ [0,n−1]
 với MaxASCIICode = 256.
 4. Nếu x và y thỏa mãn điều kiện, thì α = x và β = y. Nếu không, quay lại bước 1 và lặp lại quá trình cho đến khi tìm được α và β thích hợp.

2. Giao thức xác thực và trao đổi α, β
 Phía Alice:
 1. Chọn một số nguyên ngẫu nhiên r, sau đó thực hiện tính C1 = r ∗ G và C2 = r ∗QB +(α,β) để nhận được C = (C1,C2).
 2. Thực hiện tính h1 = hash(C|(α,β)lastsession), (α,β)lastsession là các tham số biên mã ở phiên làm việc trước.
 3. Gửi C và h1 cho Bob.

 Phía Bob:
 Khi Bob nhận được thông điệp
  1. Bob giải mã C để thu được (α,β) bằng cách tính toán:
           M =kB∗C1 =r∗kb∗G
           C2 −M =r∗QB+(α,β)−r∗kB ∗G=(α,β)
 2. Bob tính h2 = hash(C|(α,β)lastsession) và so sánh h1 với h2.
 3. Nếu h1 khớp với h2, Bob xác thực Alice và xác nhận tính toàn vẹn của (α,β). Sau đó, Bob tạo bảng ánh xạ ASCII sử dụng α và β. Ngược lại, việc xác thực Alice thất bại và Bob sẽ từ chối phiên làm việc.
 4. Bob tính: h3 = hash((α,β)) rồi thực hiện phép XOR h3 ⊕h1 =CH1
 5. Bob gửi CH1 cho Alice.

Về phía Alice:
 Khi Alice nhận được CH1:
 1. Thực hiện tính h4 = hash((α,β)).
 2. Thực hiện XOR: CH2 = h1 ⊕CH1. Nếu CH2 khớp với h4, Alice xác thực Bob và xác nhận rằng Bob đã nhận được (α,β) chính xác.
 3. Alice α và β để tạo bảng ánh xạ ASCII.
