using System;
using System.Collections.Generic;

namespace AutoSpeed
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Console.OutputEncoding = System.Text.Encoding.UTF8;

            
            Console.WriteLine("===== TC01: VALIDATION NĂM SẢN XUẤT =====");

            try
            {
                OTo otoLoi = new OTo(
                    "OT001",
                    "Toyota",
                    1850,
                    1000000000m,
                    5,
                    2.0);

                Console.WriteLine("TC01: FAIL");
                Console.WriteLine("Đối tượng vẫn được tạo!");
            }
            catch (ArgumentException ex)
            {
                Console.WriteLine("TC01: PASS");
                Console.WriteLine("Đã bắt được lỗi: " + ex.Message);
            }


           
            Console.WriteLine();
            Console.WriteLine("===== TC02: TÍNH GIÁ LĂN BÁNH Ô TÔ =====");

            OTo oto = new OTo(
                "OT002",
                "Toyota",
                2022,
                1000000000m,
                5,
                2.0);

            decimal giaOto = oto.TinhGiaLanBanh();

            Console.WriteLine(oto.GetInfo());
            Console.WriteLine(
                $"Giá lăn bánh: {giaOto:N0} VNĐ");

            if (giaOto == 1420000000m)
            {
                Console.WriteLine("TC02: PASS");
            }
            else
            {
                Console.WriteLine("TC02: FAIL");
            }


            
            Console.WriteLine();
            Console.WriteLine("===== TC03: TÍNH GIÁ LĂN BÁNH XE MÁY =====");

            XeMay xeMay = new XeMay(
                "XM001",
                "Honda",
                2022,
                50000000m,
                150);

            decimal giaXeMay = xeMay.TinhGiaLanBanh();

            Console.WriteLine(xeMay.GetInfo());
            Console.WriteLine(
                $"Giá lăn bánh: {giaXeMay:N0} VNĐ");

            if (giaXeMay == 51000000m)
            {
                Console.WriteLine("TC03: PASS");
            }
            else
            {
                Console.WriteLine("TC03: FAIL");
            }


            Console.WriteLine();
            Console.WriteLine("===== TC04: KIỂM TRA ĐA HÌNH =====");

            List<PhuongTien> danhSach =
                new List<PhuongTien>();

            danhSach.Add(oto);
            danhSach.Add(xeMay);

            foreach (PhuongTien pt in danhSach)
            {
                Console.WriteLine(pt.GetInfo());

                Console.WriteLine(
                    $"Giá lăn bánh: {pt.TinhGiaLanBanh():N0} VNĐ");

                Console.WriteLine();
            }

            Console.WriteLine("TC04: PASS");
            Console.WriteLine(
                "C# đã gọi đúng TinhGiaLanBanh() của từng loại phương tiện.");


         
            Console.WriteLine();
            Console.WriteLine("===== TC05: TÌM GIÁ LĂN BÁNH MAX =====");

            QuanLyPhuongTien quanLy =
                new QuanLyPhuongTien();

            quanLy.AddPhuongTien(oto);
            quanLy.AddPhuongTien(xeMay);

            PhuongTien max =
                quanLy.FindMaxGiaLanBanh();

            Console.WriteLine("Phương tiện có giá lăn bánh cao nhất:");

            Console.WriteLine(max.GetInfo());

            Console.WriteLine(
                $"Giá lăn bánh: {max.TinhGiaLanBanh():N0} VNĐ");

            if (max == oto &&
                max.TinhGiaLanBanh() == 1420000000m)
            {
                Console.WriteLine("TC05: PASS");
            }
            else
            {
                Console.WriteLine("TC05: FAIL");
            }


          
            Console.WriteLine();
            Console.WriteLine("===== HOÀN THÀNH KIỂM THỬ =====");
            Console.WriteLine("Nhấn phím bất kỳ để kết thúc...");

            Console.ReadKey();
        }
    }
}

using System;

namespace AutoSpeed
{
    public class OTo : PhuongTien
    {
        private int _soChoNgoi;
        private double _dungTichDongCo;

        public int SoChoNgoi
        {
            get { return _soChoNgoi; }
            set
            {
                if (value <= 0)
                    throw new ArgumentException(
                        "Số chỗ ngồi phải lớn hơn 0!");

                _soChoNgoi = value;
            }
        }

        public double DungTichDongCo
        {
            get { return _dungTichDongCo; }
            set
            {
                if (value <= 0)
                    throw new ArgumentException(
                        "Dung tích động cơ phải lớn hơn 0!");

                _dungTichDongCo = value;
            }
        }

        public OTo(
            string maPT,
            string tenHang,
            int namSanXuat,
            decimal giaGoc,
            int soChoNgoi,
            double dungTichDongCo)
            : base(maPT, tenHang, namSanXuat, giaGoc)
        {
            SoChoNgoi = soChoNgoi;
            DungTichDongCo = dungTichDongCo;
        }

        public override decimal TinhGiaLanBanh()
        {
            if (SoChoNgoi <= 9)
            {
                return GiaGoc
                     + GiaGoc * 0.12m
                     + GiaGoc * 0.30m;
            }
            else
            {
                return GiaGoc
                     + GiaGoc * 0.10m;
            }
        }

        public override string GetInfo()
        {
            return base.GetInfo()
                + $" | Số chỗ: {SoChoNgoi}"
                + $" | Dung tích động cơ: {DungTichDongCo} L";
        }
    }
}

using System;

namespace AutoSpeed
{
    public abstract class PhuongTien
    {
        private string _maPT;
        private string _tenHang;
        private int _namSanXuat;
        private decimal _giaGoc;

        public string MaPT
        {
            get { return _maPT; }
            set
            {
                if (string.IsNullOrWhiteSpace(value))
                    _maPT = "PT000";
                else
                    _maPT = value;
            }
        }

        public string TenHang
        {
            get { return _tenHang; }
            set
            {
                if (string.IsNullOrWhiteSpace(value))
                    throw new ArgumentException("Tên hãng không được để trống!");

                _tenHang = value;
            }
        }

        public int NamSanXuat
        {
            get { return _namSanXuat; }
            set
            {
                int namHienTai = DateTime.Now.Year;

                if (value < 1900 || value > namHienTai)
                    throw new ArgumentException(
                        "Năm sản xuất không hợp lệ!");

                _namSanXuat = value;
            }
        }

        public decimal GiaGoc
        {
            get { return _giaGoc; }
            set
            {
                if (value <= 0)
                    throw new ArgumentException(
                        "Giá gốc phải lớn hơn 0!");

                _giaGoc = value;
            }
        }

        public PhuongTien(
            string maPT,
            string tenHang,
            int namSanXuat,
            decimal giaGoc)
        {
            MaPT = maPT;
            TenHang = tenHang;
            NamSanXuat = namSanXuat;
            GiaGoc = giaGoc;
        }

        public abstract decimal TinhGiaLanBanh();

        public virtual string GetInfo()
        {
            return $"Mã PT: {MaPT} | " +
                   $"Hãng: {TenHang} | " +
                   $"Năm SX: {NamSanXuat} | " +
                   $"Giá gốc: {GiaGoc:N0}";
        }
    }
}

using System;
using System.Collections.Generic;

namespace AutoSpeed
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Console.OutputEncoding = System.Text.Encoding.UTF8;

            
            Console.WriteLine("===== TC01: VALIDATION NĂM SẢN XUẤT =====");

            try
            {
                OTo otoLoi = new OTo(
                    "OT001",
                    "Toyota",
                    1850,
                    1000000000m,
                    5,
                    2.0);

                Console.WriteLine("TC01: FAIL");
                Console.WriteLine("Đối tượng vẫn được tạo!");
            }
            catch (ArgumentException ex)
            {
                Console.WriteLine("TC01: PASS");
                Console.WriteLine("Đã bắt được lỗi: " + ex.Message);
            }


           
            Console.WriteLine();
            Console.WriteLine("===== TC02: TÍNH GIÁ LĂN BÁNH Ô TÔ =====");

            OTo oto = new OTo(
                "OT002",
                "Toyota",
                2022,
                1000000000m,
                5,
                2.0);

            decimal giaOto = oto.TinhGiaLanBanh();

            Console.WriteLine(oto.GetInfo());
            Console.WriteLine(
                $"Giá lăn bánh: {giaOto:N0} VNĐ");

            if (giaOto == 1420000000m)
            {
                Console.WriteLine("TC02: PASS");
            }
            else
            {
                Console.WriteLine("TC02: FAIL");
            }


            
            Console.WriteLine();
            Console.WriteLine("===== TC03: TÍNH GIÁ LĂN BÁNH XE MÁY =====");

            XeMay xeMay = new XeMay(
                "XM001",
                "Honda",
                2022,
                50000000m,
                150);

            decimal giaXeMay = xeMay.TinhGiaLanBanh();

            Console.WriteLine(xeMay.GetInfo());
            Console.WriteLine(
                $"Giá lăn bánh: {giaXeMay:N0} VNĐ");

            if (giaXeMay == 51000000m)
            {
                Console.WriteLine("TC03: PASS");
            }
            else
            {
                Console.WriteLine("TC03: FAIL");
            }


            Console.WriteLine();
            Console.WriteLine("===== TC04: KIỂM TRA ĐA HÌNH =====");

            List<PhuongTien> danhSach =
                new List<PhuongTien>();

            danhSach.Add(oto);
            danhSach.Add(xeMay);

            foreach (PhuongTien pt in danhSach)
            {
                Console.WriteLine(pt.GetInfo());

                Console.WriteLine(
                    $"Giá lăn bánh: {pt.TinhGiaLanBanh():N0} VNĐ");

                Console.WriteLine();
            }

            Console.WriteLine("TC04: PASS");
            Console.WriteLine(
                "C# đã gọi đúng TinhGiaLanBanh() của từng loại phương tiện.");


         
            Console.WriteLine();
            Console.WriteLine("===== TC05: TÌM GIÁ LĂN BÁNH MAX =====");

            QuanLyPhuongTien quanLy =
                new QuanLyPhuongTien();

            quanLy.AddPhuongTien(oto);
            quanLy.AddPhuongTien(xeMay);

            PhuongTien max =
                quanLy.FindMaxGiaLanBanh();

            Console.WriteLine("Phương tiện có giá lăn bánh cao nhất:");

            Console.WriteLine(max.GetInfo());

            Console.WriteLine(
                $"Giá lăn bánh: {max.TinhGiaLanBanh():N0} VNĐ");

            if (max == oto &&
                max.TinhGiaLanBanh() == 1420000000m)
            {
                Console.WriteLine("TC05: PASS");
            }
            else
            {
                Console.WriteLine("TC05: FAIL");
            }


          
            Console.WriteLine();
            Console.WriteLine("===== HOÀN THÀNH KIỂM THỬ =====");
            Console.WriteLine("Nhấn phím bất kỳ để kết thúc...");

            Console.ReadKey();
        }
    }
}

using System;
using System.Collections.Generic;
using System.Linq;

namespace AutoSpeed
{
    public class QuanLyPhuongTien
    {
        private List<PhuongTien> danhSach;

        public QuanLyPhuongTien()
        {
            danhSach = new List<PhuongTien>();
        }

        public void AddPhuongTien(PhuongTien pt)
        {
            if (pt == null)
                throw new ArgumentNullException(nameof(pt));

            danhSach.Add(pt);
        }

        public void DisplayAll()
        {
            if (danhSach.Count == 0)
            {
                Console.WriteLine("Danh sách phương tiện trống!");
                return;
            }

            foreach (PhuongTien pt in danhSach)
            {
                Console.WriteLine(pt.GetInfo());

                Console.WriteLine(
                    $"Giá lăn bánh: {pt.TinhGiaLanBanh():N0} VNĐ");

                Console.WriteLine("--------------------------------");
            }
        }

        public PhuongTien FindMaxGiaLanBanh()
        {
            if (danhSach.Count == 0)
                return null;

            return danhSach
                .OrderByDescending(pt => pt.TinhGiaLanBanh())
                .First();
        }

        public List<PhuongTien> SearchByName(string keyword)
        {
            if (string.IsNullOrWhiteSpace(keyword))
                return new List<PhuongTien>();

            return danhSach
                .Where(pt => pt.TenHang.Contains(
                    keyword,
                    StringComparison.OrdinalIgnoreCase))
                .ToList();
        }
    }
}

using System;

namespace AutoSpeed
{
    public class XeMay : PhuongTien
    {
        private int _dungTichXylanh;

        public int DungTichXylanh
        {
            get { return _dungTichXylanh; }
            set
            {
                if (value <= 0)
                    throw new ArgumentException(
                        "Dung tích xy-lanh phải lớn hơn 0!");

                _dungTichXylanh = value;
            }
        }

        public XeMay(
            string maPT,
            string tenHang,
            int namSanXuat,
            decimal giaGoc,
            int dungTichXylanh)
            : base(maPT, tenHang, namSanXuat, giaGoc)
        {
            DungTichXylanh = dungTichXylanh;
        }

        public override decimal TinhGiaLanBanh()
        {
            if (DungTichXylanh < 175)
            {
                return GiaGoc + GiaGoc * 0.02m;
            }
            else
            {
                return GiaGoc + GiaGoc * 0.05m;
            }
        }

        public override string GetInfo()
        {
            return base.GetInfo()
                + $" | Dung tích xy-lanh: {DungTichXylanh} cc";
        }
    }
}
<img width="1334" height="759" alt="Screenshot 2026-10-02 141106" src="https://github.com/user-attachments/assets/517ab38b-3984-4c22-a598-42eaee47e3c8" />

