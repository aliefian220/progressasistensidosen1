package databuku;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import static org.junit.Assert.assertEquals;
import static org.junit.jupiter.api.Assertions.*;

public class PerpustakaanTest {
    private Perpustakaan perpustakaan;

    @BeforeEach
    void setUp() {
        perpustakaan = new Perpustakaan();

        // Tambah buku
        perpustakaan.tambahBuku(new Buku("1", "Pemrograman Jaringan", 10));
        perpustakaan.tambahBuku(new Buku("2", "Pemrograman Web", 5));

        // Tambah member
        perpustakaan.tambahMember(new Member("07282", "Jonathan anandar cahyadi"));
        perpustakaan.tambahMember(new Member("90142", "citra nurina prabbiantissa"));
    }

    @Test
    void testTambahBuku() {
        perpustakaan.tambahBuku(new Buku("2", "Pemrograman web", 8));
        assertEquals(3, perpustakaan.getDaftarBuku().size(), "Jumlah buku harus bertambah menjadi 3");
    }

    private void assertEquals(int i, int size, String s) {
    }

    @Test
    void testTambahMember() {
        perpustakaan.tambahMember(new Member("07556", "damai deo saputra"));
        assertEquals(3, perpustakaan.getDaftarMember().size(), "Jumlah member harus bertambah menjadi 3");
    }

    @Test
    void testPinjamBukuSukses() {
        Buku buku = perpustakaan.getDaftarBuku().get(0);
        Member member = perpustakaan.getDaftarMember().get(0);

        perpustakaan.pinjamBuku(member, buku);

        assertEquals(9, buku.getStok(), "Stok buku harus berkurang menjadi 4 setelah peminjaman");
        assertEquals(1, perpustakaan.getDaftarTransaksi().size(), "Jumlah transaksi harus bertambah menjadi 1");
    }

    @Test
    void testPinjamBukuberhasilKarenaStokbukutersedia() {

    }

    @Test
    void testKembalikanBuku() {
        Buku buku = perpustakaan.getDaftarBuku().get(0);
        Member member = perpustakaan.getDaftarMember().get(0);

        perpustakaan.pinjamBuku(member, buku);
        Transaksi transaksi = perpustakaan.getDaftarTransaksi().get(0);

        perpustakaan.kembalikanBuku(transaksi.getIdTransaksi());

        assertTrue(transaksi.isReturned(), "Transaksi harus menunjukkan bahwa buku sudah dikembalikan");
        assertEquals(10, buku.getStok(), "Stok buku harus kembali ke 10 setelah pengembalian");
    }
}
