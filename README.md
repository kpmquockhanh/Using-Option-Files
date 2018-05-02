[Source](https://dev.mysql.com/doc/refman/5.7/en/option-files.html "Permalink to MySQL :: MySQL 5.7 Reference Manual :: 4.2.6 Using Option Files")

# MySQL ::Tài liệu tham khảo MySQL 5.7 :: 4.2.6 Sử dụng file tùy chọn

### 4.2.6 Sử dụng file tùy chọn

Hầu hết chương trình MySQL có thể đọc các tùy chọn khởi động từ các file tùy chọn (đôi khi được gọi là các file cấu hình). Các file tùy chọn cung cấp một cách thuận tiện để chỉ định các tùy chọn thường được sử dụng để nên không cần nhập vào dòng lệnh mỗi khi bạn chạy một chương trình.

Để xác định xem một chương trình có đọc các file tùy chọn hay không, Gọi nó với tùy chọn help `\--help`. (Đối với [**mysqld**][1], sử dụng [`\--verbose`][2] và [`\--help`][3].) Nếu chương trình đọc các file tùy chọn, thông báo --help cho biết file nào sẽ tìm và nhóm tùy chọn nào được nó nhận ra.

Chú thích 

Một chương trình MySQL bắt đầu với tùy chọn `\--no-defaults` chỉ đọc tùy chọn trong `.mylogin.cnf`. 

Nhiều file tùy chọn là các file văn bản thuần, được tạo bằng bất kỳ trình soạn thảo văn bản nào. Ngoại lệ là file `.mylogin.cnf` có chứa các tùy chọn đường dẫn đăng nhập. Đây là file được mã hóa bởi tiện ích [**mysql_config_editor**][4]. Xem [Section 4.6.6, "**mysql_config_editor** — Tiện ích cấu hình MySQL"][4]. Một "đường dẫn đăng nhập" là nhóm tùy chọn chỉ cho phép một số tùy chọn nhất định: `host`, `user`, `password`, `port` và `socket`. Chương trình khách sử dụng tùy chọn [`\--login-path`][5] để xác định xem sẽ đọc đường dẫn đăng nhập nào từ `.mylogin.cnf`.

Để chỉ định tên file đường dẫn đăng nhập thay thế, hãy đặt biến môi trường `MYSQL_TEST_LOGIN_FILE`. Biến này được sử dụng bởi tiện ích kiểm thử **mysql-test-run.pl**, nhưng cũng được nhận biết bởi [**mysql_config_editor**][4] và bởi MySQL khách như là [**mysql**][6], [**mysqladmin**][7], và hơn nữa. 

MySQL tìm kiếm các file tùy chọn theo thứ tự được mô tả trong phần thảo luận dưới và đọc bất kỳ file nào tồn tại. Nếu một file tùy chọn bạn muốn sử dụng không tồn tại, hãy tạo nó bằng cách sử dụng phương thức thích hợp, như đã được đề cập như trên. 

Trên Windows, Chương trình MySQL đọc các tùy chọn khởi động từ các file được hiển thị trong bảng sau, theo thứ tự được chỉ định (các file được liệt kê đầu tiên được đọc trước tiên, các file sau được đọc theo thứ tự ưu tiên).

**Table 4.1 File tùy chọn Đọc trên các hệ thống Windows**

| File Name                                                                                  | Purpose                                                       |  
| ------------------------------------------------------------------------------------------ | ------------------------------------------------------------- |  
| ``%PROGRAMDATA%`MySQLMySQL Server 5.7my.ini`, ``%PROGRAMDATA%`MySQLMySQL Server 5.7my.cnf` | Global options                                                |  
| ``%WINDIR%`my.ini`, ``%WINDIR%`my.cnf`                                                     | Global options                                                |  
| `C:my.ini`, `C:my.cnf`                                                                     | Global options                                                |  
| `_`BASEDIR`_my.ini`, `_`BASEDIR`_my.cnf`                                                   | Global options                                                |  
| `defaults-extra-file`                                                                      | The file specified with [`\--defaults-extra-file`][8], if any |  
| ``%APPDATA%`MySQL.mylogin.cnf`                                                             | Login path options (clients only)                             |  

Trong bảng trên, `% PROGRAMDATA%` đại diện cho thư mục hệ thống file chứa dữ liệu ứng dụng cho tất cả người dùng trên máy chủ lưu trữ. Đường dẫn này mặc định là `C: ProgramData` trên Microsoft Windows Vista trở lên và` C: Documents and SettingsAll UsersApplication Data` trên các phiên bản cũ hơn của Microsoft Windows. 

`% WINDIR%` đại diện cho vị trí của thư mục Windows của bạn. Điều này thường là `C: WINDOWS`. Sử dụng lệnh sau đây để xác định vị trí chính xác của nó từ giá trị của biến môi trường `WINDIR`:
    
    
    C:> echo %WINDIR%

`% APPDATA%` đại diện cho giá trị của thư mục dữ liệu ứng dụng Windows. Sử dụng lệnh sau đây để xác định vị trí chính xác của nó từ giá trị của biến môi trường `APPDATA`:
    
    
    C:> echo %APPDATA%

_`BASEDIR`_ đại diện cho thư mục cài đặt cơ sở MySQL. Khi MySQL 5.7 đã được cài đặt bằng cách sử dụng MySQL Installer, đây thường là `C: _`PROGRAMDIR`_MySQLMySQL 5.7 Server` trong đó _`PROGRAMDIR`_ đại diện cho thư mục chương trình (thường là` Program Files` trên các phiên bản tiếng Anh của Windows), [Phần 2.3.3, "Trình cài đặt MySQL cho Windows"] [9]. 

Trên các hệ thống giống Unix và Unix, các chương trình MySQL đọc các tùy chọn khởi động từ các file được hiển thị trong bảng sau, theo thứ tự được chỉ định (các file được liệt kê đầu tiên được đọc trước tiên, các file sau được đọc theo thứ tự ưu tiên). 

Chú thích 

Trên các nền tảng Unix, MySQL bỏ qua các file cấu hình có thể ghi trên thế giới. Đây là cố ý như một biện pháp an ninh. 

**Bảng 4.2 Các tệp tùy chọn được đọc trên các hệ thống giống Unix và Unix**

| File Name               | Purpose                                                       |  
| ----------------------- | ------------------------------------------------------------- |  
| `/etc/my.cnf`           | Global options                                                |  
| `/etc/mysql/my.cnf`     | Global options                                                |  
| `_`SYSCONFDIR`_/my.cnf` | Global options                                                |  
| `$MYSQL_HOME/my.cnf`    | Server-specific options (server only)                         |  
| `defaults-extra-file`   | The file specified with [`\--defaults-extra-file`][8], if any |  
| `~/.my.cnf`             | User-specific options                                         |  
| `~/.mylogin.cnf`        | User-specific login path options (clients only)               |  

Trong bảng trước, `~` đại diện cho thư mục chính của người dùng hiện tại (giá trị của `$ HOME`). 

_`SYSCONFDIR`_ đại diện cho thư mục được chỉ định với tùy chọn [`SYSCONFDIR`] [10] để ** CMake ** khi MySQL được xây dựng. Theo mặc định, đây là thư mục `etc` nằm trong thư mục cài đặt đã biên dịch. 

`MYSQL_HOME` là một biến môi trường chứa đường dẫn đến thư mục chứa file` my.cnf` cụ thể của máy chủ. Nếu `MYSQL_HOME` không được đặt và bạn khởi động máy chủ bằng chương trình [** mysqld_safe **] [11], [** mysqld_safe **] [11] đặt nó thành _`BASEDIR`_, thư mục cài đặt cơ sở MySQL . 

_`DATADIR`_ thường là `/ usr / local / mysql / data`, mặc dù điều này có thể thay đổi theo từng nền tảng hoặc phương pháp cài đặt. Giá trị là vị trí thư mục dữ liệu được xây dựng trong khi MySQL được biên dịch, không phải là vị trí được chỉ định với tùy chọn [`\ - datadir`] [12] khi [** mysqld **] [1] bắt đầu. Sử dụng [`\ - datadir`] [12] khi chạy không ảnh hưởng đến nơi máy chủ tìm kiếm các file tùy chọn mà nó đọc trước khi xử lý bất kỳ tùy chọn nào.

Nếu tìm thấy nhiều cá thể của một tùy chọn nhất định, cá thể cuối cùng được ưu tiên, với một ngoại lệ: Đối với ** [mysqld **] [1], cá thể _first_ của tùy chọn `[\ - user`] [13] là được sử dụng như một biện pháp phòng ngừa an ninh, để ngăn người dùng được chỉ định trong một file tùy chọn bị ghi đè trên dòng lệnh. 

Mô tả sau đây về cú pháp file tùy chọn áp dụng cho các file bạn chỉnh sửa theo cách thủ công. Điều này không áp dụng với `.mylogin.cnf`, được tạo ra bằng cách sử dụng ** [mysql_config_editor **] [4] và được mã hóa. 

Bất kỳ tùy chọn dài có thể được đưa ra trên dòng lệnh khi chạy một chương trình MySQL tốt hơn là đưa ra trong một tập tin tùy chọn. Để có được danh sách các tùy chọn có sẵn cho một chương trình, hãy chạy nó với tùy chọn `\ - help`. (Đối với ** [mysqld **] [1], sử dụng `[\ - verbose`] [2] và` [\ - help`] [3].)

Cú pháp để chỉ định các tùy chọn trong một file tùy chọn tương tự như cú pháp dòng lệnh (xem [Phần 4.2.4, "Sử dụng Tùy chọn trên Dòng Lệnh"] [14]). Tuy nhiên, trong một file tùy chọn, bạn bỏ qua hai dấu gạch ngang hàng đầu từ tên tùy chọn và bạn chỉ định một tùy chọn trên mỗi dòng. Ví dụ, `\ - quick` và` \ - host = localhost` trên dòng lệnh nên được chỉ định là `quick` và` host = localhost` trên các dòng riêng biệt trong một file tùy chọn. Để chỉ định một tùy chọn của biểu mẫu `\ - loose-_`opt_name`_` trong một file tùy chọn, hãy viết nó như là` loose-_`opt_name`_`. 

Các dòng trống trong các file tùy chọn bị bỏ qua. Các dòng không trống có thể có các dạng sau đây:

* `#_`comment`_`, `;_`comment`_`

Dòng chú thích bắt đầu bằng `#` hoặc `;`. Nhận xét `#` cũng có thể bắt đầu ở giữa một dòng. 

* `[_`group`_]`

_`group`_ là tên của chương trình hay của nhóm mà bạn muốn cài các tùy chọn. Sau 1 nhóm dòng, bất kỳ dòng cài-đặt-tùy-chọn được áp dụng cho nhóm đã được nêu tên cho tới khi kết thúc của file tùy chọn hoặc 1 dòng nhóm khác được đưa ra. Tên của các nhóm tùy chọn không phân biệt hoa thường.

* `_`opt_name`_`

Cái này tương đương với `\--_`opt_name`_` trên dòng lệnh. 

* `_`opt_name`_=_`value`_`

Cái này tương đương với `\ --_` opt_name` _ = _ `value`_` trên dòng lệnh. Trong một file tùy chọn, bạn có thể có khoảng trống xung quanh ký tự `=`, một vài cái lại không đúng trên dòng lệnh. Giá trị tùy chọn có thể được đặt trong dấu nháy đơn hoặc dấu ngoặc kép, rất hữu ích nếu giá trị chứa ký tự nhận xét `#`. 

Các khoảng trống phía trước và sau sẽ tự động được xóa khỏi tên và các giá trị tùy chọn. 

Bạn có thể sử dụng các escape sequence `b`, `t`, `n`, `r`, `\`,và `s` trong các giá trị tùy chọn để đại diện cho các kí tự backspace, tab, newline, carriage return, backslash, và space. Trong các file tùy chọn, những luật escaping sau được áp dụng 

* 1 kí tự backslash theo sau bởi 1 kí tự escape sequence hợp lệ sẽ được chuyển thành kí tự đại diện bởi sequence. Ví dụ, `s` sẽ được chuyển thành khoảng trắng. 
* 1 kí tự backflash không theo sau bởi 1 kí tự escape sequence hợp lệ sẽ không vì thay đổi. Ví dụ `S` vẫn giữ nguyên là nó. 

Các luật trên có nghĩa là 1 kí tự backslash đơn thuần có thể được đưa ra bởi \, hoặc như là `` nếu nó không được theo sau bởi kí tự escape sequence hợp lệ. 

Các quy tắc cho các escape sequence trong các file tùy chọn khác nhau một chút so với các quy tắc cho các escape sequence trong các chuỗi ký tự trong các câu lệnh SQL. Trong bối cảnh sau, nếu "_`x`_" không phải là ký tự thứ tự thoát hợp lệ, `_`x`_` trở thành" _`x`_ "thay vì` _`x`_`. Xem [Mục 9.1.1, "Chuỗi văn bản"] [15]. 

Các luật escaping cho các giá trị file tùy chọn đặc biệt thích hợp cho các tên đường dẫn Windows, nơi sử dụng `` để phân chia tên đường dẫn. 1 thành phần phân chia trong tên đường dẫn Windows phải được viết là `\` enếu nó được theo sau bởi kí tự escape sequence. Nó có thể được viết là `\` hoặc nếu không thì là ``. Ngoài ra, `\` có thể được sử dụng trong các tên đường dẫn Windows và có thể được coi như là ``. Giả sử rằng bạn muốn chỉ định thư mục gốc của `C:Program FilesMySQLMySQL Server 5.7` trong 1 file tùy chọn. Điều này có thể được thực hiện bằng 1 vài cách. 1 vài ví dụ: 
    
    
    basedir="C:Program FilesMySQLMySQL Server 5.7"
    basedir="C:\Program Files\MySQL\MySQL Server 5.7"
    basedir="C:/Program Files/MySQL/MySQL Server 5.7"
    basedir=C:\ProgramsFiles\MySQL\MySQLsServers5.7

Nếu tên nhóm tùy chọn giống với tên chương trình, các tùy chọn trong nhóm sẽ áp dụng riêng cho chương trình đó. Ví dụ, các nhóm `[mysqld]` và `[mysql]` áp dụng cho máy chủ ** [mysqld **] [1] và chương trình máy khách ** [mysql **] [6], tương ứng. 

Nhóm tùy chọn `[client]` được đọc bởi tất cả các chương trình máy khách được cung cấp trong các bản phân phối MySQL (nhưng _not_ bởi ** [mysqld **] [1]). Để hiểu cách các chương trình khách hàng của bên thứ ba sử dụng API C có thể sử dụng các file tùy chọn, hãy xem tài liệu C API tại [Mục 27.8.7.50, "mysql_options ()"] [16].

Nhóm `[client]` cho phép bạn chỉ định các tùy chọn áp dụng cho tất cả các máy khách. Ví dụ, `[client]` là nhóm thích hợp để sử dụng để chỉ định mật khẩu để kết nối với máy chủ. (Nhưng hãy chắc chắn rằng tập tin tùy chọn chỉ có thể truy cập được bởi chính bạn, để người khác không thể tìm ra mật khẩu của bạn.) Đảm bảo không đặt tùy chọn trong nhóm `[client]` trừ khi nó được các chương trình khách _all_ nhận ra mà bạn sử dụng . Các chương trình không hiểu tùy chọn thoát sau khi hiển thị thông báo lỗi nếu bạn cố gắng chạy chúng. 

Liệt kê các nhóm tùy chọn chung hơn trước tiên và các nhóm cụ thể hơn sau này. Ví dụ, một nhóm `[client]` là tổng quát hơn vì nó được đọc bởi tất cả các chương trình máy khách, trong khi nhóm `[mysqldump]` chỉ được đọc bởi ** [mysqldump **] [17]. Tùy chọn được chỉ định sau này ghi đè các tùy chọn được chỉ định trước đó, do đó, đặt các nhóm tùy chọn theo thứ tự `[client]`, `[mysqldump]` cho phép ** [mysqldump **] [17] -specific options để ghi đè các tùy chọn `[client]`. 

Đây là một file tùy chọn toàn cục điển hình: 
    
    
    [client]
    port=3306
    socket=/tmp/mysql.sock
    
    [mysqld]
    port=3306
    socket=/tmp/mysql.sock
    key_buffer_size=16M
    max_allowed_packet=8M
    
    [mysqldump]
    quick

Đây là một file tùy chọn người dùng điển hình:
    
    
    [client]
    # The following password will be sent to all standard MySQL clients
    password="my password"
    
    [mysql]
    no-auto-rehash
    connect_timeout=2

Để tạo các nhóm tùy chọn chỉ đọc bởi các máy chủ ** [mysqld **] [1] từ loạt phát hành MySQL cụ thể, hãy sử dụng các nhóm có tên `[mysqld-5.6]`, `[mysqld-5.7]`, v.v. . Nhóm sau chỉ ra rằng cài đặt `[sql_mode`] [18] chỉ nên được sử dụng bởi các máy chủ MySQL có số phiên bản 5.7.x: 
    
    
    [mysqld-5.7]
    sql_mode=TRADITIONAL

Có thể sử dụng các chỉ thị `! Include` trong các tfileệp tùy chọn để bao gồm các file tùy chọn khác và`! Includeir` để tìm kiếm các thư mục cụ thể cho các file tùy chọn. Ví dụ: để bao gồm file `/ home / mydir / myopt.cnf`, hãy sử dụng chỉ thị sau: 
    
    
    !include /home/mydir/myopt.cnf

Để tìm kiếm thư mục `/ home / mydir` và đọc các file tùy chọn được tìm thấy ở đó, hãy sử dụng chỉ thị này: 
    
    
    !includedir /home/mydir

MySQL không đảm bảo về thứ tự các tập tin tùy chọn trong thư mục sẽ được đọc. 

Chú thích 

Bất kỳ file nào được tìm thấy và bao gồm bằng cách sử dụng chỉ thị `! Includeir` trên hệ điều hành Unix _must_ có tên file kết thúc bằng` .cnf`. Trên Windows, chỉ thị này kiểm tra các file có phần mở rộng `.ini` hoặc` .cnf`. 

Viết nội dung của file tùy chọn được bao gồm giống như bất kỳ file tùy chọn nào khác. Tức là, nó phải chứa các nhóm tùy chọn, mỗi nhóm đứng trước một dòng `[_`group`_]` chỉ ra chương trình mà các tùy chọn áp dụng. 

Trong khi file được bao gồm đang được xử lý, chỉ những tùy chọn trong nhóm mà chương trình hiện tại đang tìm kiếm được sử dụng. Các nhóm khác bị bỏ qua. Giả sử rằng file `my.cnf` chứa dòng này: 
    
    
    !include /home/mydir/myopt.cnf

Và giả sử rằng `/ home / mydir / myopt.cnf` giống như sau:
    
    
    [mysqladmin]
    force
    
    [mysqld]
    key_buffer_size=16M

Nếu `my.cnf` được xử lý bởi ** [mysqld **] [1], chỉ có nhóm` [mysqld] `trong` / home / mydir / myopt.cnf` được sử dụng. Nếu file được xử lý bởi ** [mysqladmin **] [7] thì chỉ sử dụng nhóm `[mysqladmin]`. Nếu file được xử lý bởi bất kỳ chương trình nào khác, không có tùy chọn nào trong `/ home / mydir / myopt.cnf` được sử dụng. 

Chỉ thị `! Includeir` được xử lý tương tự ngoại trừ tất cả các file tùy chọn trong thư mục có tên được đọc.

Nếu một file tùy chọn chứa các chỉ thị `! Include` hoặc`! Includeir`, các file được đặt tên bởi các chỉ thị đó được xử lý bất cứ khi nào file tùy chọn được xử lý, bất kể chúng xuất hiện ở đâu trong file.

[1]: https://dev.mysql.com/mysqld.html "4.3.1 mysqld — The MySQL Server"
[2]: https://dev.mysql.com/server-options.html#option_mysqld_verbose
[3]: https://dev.mysql.com/server-options.html#option_mysqld_help
[4]: https://dev.mysql.com/mysql-config-editor.html "4.6.6 mysql_config_editor — MySQL Configuration Utility"
[5]: https://dev.mysql.com/option-file-options.html#option_general_login-path
[6]: https://dev.mysql.com/mysql.html "4.5.1 mysql — The MySQL Command-Line Tool"
[7]: https://dev.mysql.com/mysqladmin.html "4.5.2 mysqladmin — Client for Administering a MySQL Server"
[8]: https://dev.mysql.com/option-file-options.html#option_general_defaults-extra-file
[9]: https://dev.mysql.com/mysql-installer.html "2.3.3 MySQL Installer for Windows"
[10]: https://dev.mysql.com/source-configuration-options.html#option_cmake_sysconfdir
[11]: https://dev.mysql.com/mysqld-safe.html "4.3.2 mysqld_safe — MySQL Server Startup Script"
[12]: https://dev.mysql.com/server-options.html#option_mysqld_datadir
[13]: https://dev.mysql.com/server-options.html#option_mysqld_user
[14]: https://dev.mysql.com/command-line-options.html "4.2.4 Using Options on the Command Line"
[15]: https://dev.mysql.com/string-literals.html "9.1.1 String Literals"
[16]: https://dev.mysql.com/mysql-options.html "27.8.7.50 mysql_options()"
[17]: https://dev.mysql.com/mysqldump.html "4.5.4 mysqldump — A Database Backup Program"
[18]: https://dev.mysql.com/server-system-variables.html#sysvar_sql_mode
