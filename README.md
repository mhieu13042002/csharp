using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace giuaky
{
    public class MayTinh
    {
        public string MS;
        public string Tenmaytinh;
        public string CPU;
        public string Main;
        public string Ram;
        public string Nhacungcap;
        public string Nhasanxuat;
        public float DiemDanhgia;
        public int SoLuong;
        public float Gia;
    }

    public class Node
    {
        public MayTinh data;
        public Node next;
        public Node(MayTinh mt)
        {
            data = mt;
            next = null;
        }
    }
    public class DanhSachMayTinh
    {
        private Node head;

        public void DuLieu()
        {
            List<MayTinh> danhSach = new List<MayTinh>
    {
        new MayTinh { MS = "MT01", Tenmaytinh = "Laptop Dell", CPU = "i5", Main = "Dell Main", Ram = "8GB", Nhacungcap = "FPT", Nhasanxuat = "Dell", DiemDanhgia = 4.5f, SoLuong = 10, Gia = 15 },
        new MayTinh { MS = "MT02", Tenmaytinh = "PC Gaming", CPU = "i7", Main = "ASUS", Ram = "16GB", Nhacungcap = "NguyenKim", Nhasanxuat = "Custom", DiemDanhgia = 4.8f, SoLuong = 5, Gia = 20 },
        new MayTinh { MS = "MT03", Tenmaytinh = "Laptop Asus", CPU = "i3", Main = "Asus Main", Ram = "4GB", Nhacungcap = "PhongVu", Nhasanxuat = "Asus", DiemDanhgia = 3.9f, SoLuong = 12, Gia = 12 },
        new MayTinh { MS = "MT04", Tenmaytinh = "Laptop HP", CPU = "i5", Main = "HP Main", Ram = "8GB", Nhacungcap = "AnPhat", Nhasanxuat = "HP", DiemDanhgia = 4.2f, SoLuong = 8, Gia = 17 },
        new MayTinh { MS = "MT05", Tenmaytinh = "Laptop Lenovo", CPU = "i7", Main = "Lenovo Main", Ram = "16GB", Nhacungcap = "TGDĐ", Nhasanxuat = "Lenovo", DiemDanhgia = 4.7f, SoLuong = 15, Gia = 19 }
    };

            foreach (var mt in danhSach)
            {
                ThemTheoThuTu(mt);
            }
        }


        public void ThemTheoThuTu(MayTinh mt)
        {
            if (TrungKhoa(mt.MS)) return;

            Node newNode = new Node(mt);

            if (head == null || string.Compare(mt.MS, head.data.MS) < 0)
            {
                newNode.next = head;
                head = newNode;
                return;
            }

            Node temp = head;
            while (temp.next != null && string.Compare(temp.next.data.MS, mt.MS) < 0)
            {
                temp = temp.next;
            }

            newNode.next = temp.next;
            temp.next = newNode;
        }

        private bool TrungKhoa(string ms)
        {
            Node temp = head;
            while (temp != null)
            {
                if (temp.data.MS == ms) return true;
                temp = temp.next;
            }
            return false;
        }

        public void TimTheoMS(string ms)
        {
            Node temp = head;
            while (temp != null)
            {
                if (temp.data.MS == ms)
                {
                    XuatMayTinh(temp.data);
                    return;
                }
                temp = temp.next;
            }
            Console.WriteLine("Khong tim thay.");
        }

        public void TimTheoKhoangGia(float min, float max)
        {
            Node temp = head;
            while (temp != null)
            {
                if (temp.data.Gia >= min && temp.data.Gia <= max)
                {
                    XuatMayTinh(temp.data);
                }
                temp = temp.next;
            }
        }

        public void SapXepTheoTen()
        {
            for (Node i = head; i != null; i = i.next)
            {
                for (Node j = i.next; j != null; j = j.next)
                {
                    if (string.Compare(i.data.Tenmaytinh, j.data.Tenmaytinh) > 0)
                    {
                        MayTinh temp = i.data;
                        i.data = j.data;
                        j.data = temp;
                    }
                }
            }
        }

        public void XoaTheoTen(string ten)
        {
            while (head != null && head.data.Tenmaytinh == ten)
                head = head.next;

            Node current = head;
            while (current != null && current.next != null)
            {
                if (current.next.data.Tenmaytinh == ten)
                    current.next = current.next.next;
                else
                    current = current.next;
            }
        }

        public DanhSachMayTinh TaoDanhSachGiamTheoDiem()
        {
            DanhSachMayTinh newList = new DanhSachMayTinh();
            Node current = head;
            while (current != null)
            {
                newList.ChenGiamTheoDiem(current.data);
                current = current.next;
            }
            return newList;
        }

        public void ChenGiamTheoDiem(MayTinh mt)
        {
            Node newNode = new Node(mt);

            if (head == null || mt.DiemDanhgia >= head.data.DiemDanhgia)
            {
                newNode.next = head;
                head = newNode;
                return;
            }

            Node temp = head;
            while (temp.next != null && temp.next.data.DiemDanhgia > mt.DiemDanhgia)
            {
                temp = temp.next;
            }

            newNode.next = temp.next;
            temp.next = newNode;
        }

        public void MayTinhDiemCaoNhat()
        {
            if (head == null) return;
            Node max = head;
            Node temp = head.next;
            while (temp != null)
            {
                if (temp.data.DiemDanhgia > max.data.DiemDanhgia)
                    max = temp;
                temp = temp.next;
            }
            Console.WriteLine("May tinh co diem danh gia cao nhat:");
            XuatMayTinh(max.data);
        }

        public void MayTinhDiemThapNhat()
        {
            if (head == null) return;
            Node min = head;
            Node temp = head.next;
            while (temp != null)
            {
                if (temp.data.DiemDanhgia < min.data.DiemDanhgia)
                    min = temp;
                temp = temp.next;
            }
            Console.WriteLine("May tinh co diem danh gia thap nhat:");
            XuatMayTinh(min.data);
        }

        public void LaptopMuaNhieuNhat()
        {
            Node temp = head;
            Node max = null;
            while (temp != null)
            {
                if (temp.data.Tenmaytinh.ToLower().Contains("laptop"))
                {
                    if (max == null || temp.data.SoLuong > max.data.SoLuong)
                        max = temp;
                }
                temp = temp.next;
            }

            if (max != null)
            {
                Console.WriteLine("Laptop duoc mua nhieu nhat:");
                XuatMayTinh(max.data);
            }
            else
            {
                Console.WriteLine("Khong co laptop nao trong danh sach.");
            }
        }

        public void XuatDanhSach()
        {
            Node temp = head;
            while (temp != null)
            {
                XuatMayTinh(temp.data);
                temp = temp.next;
            }
        }

        private void XuatMayTinh(MayTinh mt)
        {
            Console.WriteLine($"MS: {mt.MS} | Ten: {mt.Tenmaytinh} | CPU: {mt.CPU} | Main: {mt.Main} | Ram: {mt.Ram} | NCC: {mt.Nhacungcap} | NSX: {mt.Nhasanxuat} | Diem: {mt.DiemDanhgia} | SL: {mt.SoLuong} | Gia: {mt.Gia}tr");
        }
    }


    internal class Program
    {
        static void Main(string[] args)
        {
            DanhSachMayTinh ds = new DanhSachMayTinh();
            ds.DuLieu();

            Console.WriteLine("\n--- Danh sach may tinh:");
            ds.XuatDanhSach();

            Console.WriteLine("\n--- Tim may theo MS:");
            ds.TimTheoMS("MT01");

            Console.WriteLine("\n--- May trong khoang gia 12-20 trieu:");
            ds.TimTheoKhoangGia(12, 20);

            Console.WriteLine("\n--- Sap xep theo ten:");
            ds.SapXepTheoTen();
            ds.XuatDanhSach();

            Console.WriteLine("\n--- Xoa may tinh ten 'Laptop Asus':");
            ds.XoaTheoTen("Laptop Asus");
            ds.XuatDanhSach();

            Console.WriteLine("\n--- Danh sach moi theo diem danh gia giam dan:");
            DanhSachMayTinh dsMoi = ds.TaoDanhSachGiamTheoDiem();
            dsMoi.XuatDanhSach();

            Console.WriteLine("\n--- May tinh co diem cao nhat:");
            ds.MayTinhDiemCaoNhat();

            Console.WriteLine("\n--- May tinh co diem thap nhat:");
            ds.MayTinhDiemThapNhat();

            Console.WriteLine("\n--- Laptop duoc mua nhieu nhat:");
            ds.LaptopMuaNhieuNhat();
            Console.ReadKey();
        }
        
    }
    
}
    

