# Nhật ký tự học Solution Architecture Associate AWS - Week 1 (4/10/2021 - 10/10/2021)

![SAA-badge](../images/SAA/AWS-Certified_Solutions-Architect_Associate_badge.png)

## Những thứ mình đã học được trong một tuần qua:

### 1st Pillar: Kiến trúc an toàn (Secure Architecture)

Một kiến trúc an toàn là kiến trúc có khả năng bảo vệ thông tin, hệ thống, tài nguyên, dữ liệu trên mây của bạn một cách đáng tin cậy và an toàn, trong khi vẫn cho phép bạn sử dụng tât cả tài nguyên này để phục vụ mục đích kinh doanh

Có 3 mối quan tâm chính khi xây dựng kiến trúc an toàn:
+ Thiết kế các cách truy cập an toàn tới tài nguyên trên AWS
+ Thiết kế các lớp ứng dụng an toàn
+ Lựa chọn các phương án bảo mật thông tin phù hợp

1. Thiết kế các cách truy cập an toàn
**keywords**: principal, user, group, role, IAM, policy, credentials, 

Một principal là một đối tượng có quyền truy cập đến tài nguyên AWS (user, role,...)
Các đối tượng này khi mới khởi tạo sẽ không có quyền gì, cho đến khi được gán IAM policy

Một user là một đối tượng tồn tại lâu dài trong hệ thống (người, chương trình...), ít nhất đến khi IAM admin xóa user này. User có quyền truy cập hệ tài nguyên qua các loại xác thực sau:
+ Truy cập đến giao diện quản lý thông qua username/pass
+ Truy cập đến tài nguyên thông qua API trong CLI, SDK (Programmatic access) thông qua Access key. Access key bao gồm *Access key ID* và *Secret Access Key* (có thể bao gồm *Session key*).
**best practices:**
+ Tạo user có đúng quyền hạn (least privileges)

Root user có quyền lực tối cao và không hạn chế trong AWS.
**best practices:**
+ Xóa access key của Root sau khi khởi tạo
+ Không sử dụng tài khoản này trong task hằng ngày
+ sử dụng Multifactor Authentication (MFA)

Group cho phép chứa nhiều role, thuận tiện khi số user quá nhiều, không thể tạo và gắn policy riêng lẻ cho từng user

Role cho phép một đối tượng có quyền truy cập đến tài nguyên được quy định trong một thời hạn cố định. AWS sẽ cung cấp token tạm thời cho đối tượng được gắn role, token sẽ hết hạn sau một khoảng thời gian quy định
**best practices:**
+ Sử dụng role thay cho user khi muốn cấp quyền tạm thời cho chương trình truy cập đến AWS. Điều này sẽ giúp không phải lưu Access key trong file config (~/.aws/credentials với linux) hay hard-code credential trong môi trường, tránh rủi ro access key của user bị lộ ra ngoài.

Để cấp quyền  truy cập cho user/role/group, tạo và gán IAM Policy cho những đối tượng đó. IAM policy đươc viết bằng json, trong đó có 4 mục quan trọng nhất:
+ Effect: cho phép hay từ chối
+ Action:
+ Resource:
+ Condition


