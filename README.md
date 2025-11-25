-- Tạo database nếu chưa có
IF NOT EXISTS (SELECT * FROM sys.databases WHERE name = 'QuanLyThuVien_QuocAn')
    CREATE DATABASE QuanLyThuVien_QuocAn;
GO

USE QuanLyThuVien_QuocAn;
GO

-- Tạo bảng Sach
CREATE TABLE Sach(
    MaSach CHAR(10) PRIMARY KEY,
    TenSach NVARCHAR(50),
    SoLuong INT
);
GO

-- Tạo bảng DocGia
CREATE TABLE DocGia(
    SoThe CHAR(10) PRIMARY KEY,
    HoTen NVARCHAR(50),
    GioiTinh bit,
    NgaySinh DATE,
    DiaChi NVARCHAR(100),
    SDT VARCHAR(15)
);
GO

-- Tạo bảng SachMuon
CREATE TABLE SachMuon(
    MaSach CHAR(10),
    MaDocGia CHAR(10),
    NgayMuon DATE,
    PRIMARY KEY (MaSach, MaDocGia),
    FOREIGN KEY (MaSach) REFERENCES Sach(MaSach),
    FOREIGN KEY (MaDocGia) REFERENCES DocGia(SoThe)
);
GO
-- Chèn dữ liệu vào Sach
INSERT INTO Sach(MaSach, TenSach, SoLuong)
VALUES 
('S001', N'Đắc Nhân Tâm', 8),
('S002', N'Exodia Can Trường', 10),
('S003', N'Dũng Sĩ Lesin', 20);
GO

-- Chèn dữ liệu vào DocGia
INSERT INTO DocGia(SoThe, HoTen, GioiTinh,DiaChi, NgaySinh, SDT)
VALUES
('K001', N'Trần Duy Huy', 0, N'Vĩnh Phúc','2005-10-15',NULL),
('K002', N'VoleyBear', 0, N'Hà Nội','2007-7-8', '0838482340'),
('K003', N'Phạm Hà', 1, N'Hưng Yên', '2006-10-15','0495883234');
GO
SELECT
SoThe ,
HoTen ,
case GioiTinh
when 0 then N'Nữ'
when 1 then N'Nam'
end as GioiTinh,
NgaySinh,
DiaChi,
SDT
from DocGia

-- Chèn dữ liệu vào SachMuon (bỏ khoảng trắng thừa)
INSERT INTO SachMuon(MaSach, MaDocGia, NgayMuon)
VALUES
('S001', 'K001', '2025-11-20'),
('S002', 'K001', '2025-11-20'),
('S003', 'K001', '2024-11-20'),
('S001', 'K003', '2025-11-25'),
('S003', 'K002', '2024-10-15'),
('S002', 'K002', '2025-09-02');
GO

-- Hiển thị dữ liệu
SELECT * FROM Sach;
SELECT * FROM DocGia;
SELECT * FROM SachMuon;
GO

-- Cập nhật dữ liệu
UPDATE DocGia
SET HoTen = N'Quốc An',
    SDT = '098765432'
WHERE SoThe = 'K002';
GO

-- Kiểm tra lại dữ liệu sau cập nhật
SELECT N'Dữ liệu Độc Giả sau khi cập nhật:' AS Bang;
SELECT * FROM DocGia;

--bài 2
select 
HoTen as[ Tên độc giả],
Year(getdate())-year(NgaySinh) as [Tuổi],
SDT as[ Số điện thoại]
from DocGia
where 
SDT Is Null
Go
select
S.TenSach as[Tên Sách],
DG.HoTen as[ Tên độc giả nữ],
SM.NgayMuon
from SachMuon SM
join Sach S on SM.MaSach= S.MaSach
join DocGia DG on SM.MaDocGia= DG.SoThe
where 
DG.GioiTinh=0
and Year(SM.NgayMuon)= 2025
go
select top 1 with TIES
DG.HoTen as[Họ tên độc giả]
from SachMuon SM
join DocGia DG on SM.MaDocGia= DG.SoThe
group by
DG.HoTen
order by
count(SM.MaSach)desc
go
