# UTS_Pemograman-Visual-Dekstop
Pada praktikum ini, kita akan membuat Project baru dengan nama LatihanCRUD, dengan memakai Database pv_biodata dan jFrame seperti jFrame Biodata dengan beberapa perubahan desain :

![0111](https://github.com/user-attachments/assets/a552685d-4322-415c-ac84-675b3956341c)

![image](https://github.com/user-attachments/assets/15fd1232-7d7e-4c9e-a760-baffadbe3b89)

<img width="946" alt="2" src="https://github.com/user-attachments/assets/add58cdd-9dc5-414c-8bf0-042bc04733af">
Buatlah Project baru dengan nama LatihanCRUD.
<img width="461" alt="3" src="https://github.com/user-attachments/assets/e5a63e21-50d2-4858-a3f8-8423b8031f35">
<img width="505" alt="4" src="https://github.com/user-attachments/assets/4f5dd79d-18dd-4726-b65c-4fa1aebda86d">
Klik kanan Libraries → Add JAR/Folder...
<img width="569" alt="5" src="https://github.com/user-attachments/assets/e96effee-b1b7-439c-b1e5-fd3507da9389">
Buatlah 2 buah Package dengan nama Config dan Form, klik kanan Source Packages → New → Java Package...
<img width="500" alt="6" src="https://github.com/user-attachments/assets/948d97ac-84de-4037-94f4-8ed45565966a">
Pada Package Config, bualah Java Class baru dengan nama KoneksiDB
<img width="569" alt="7" src="https://github.com/user-attachments/assets/9f6156b6-4d65-48a8-9ce3-dccd6775182d">

Skrip KoneksiDB.java

/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 * Click nbfs://nbhost/SystemFileSystem/Templates/Classes/Class.java to edit this template
 */
package config;

import java.sql.Connection;
import java.sql.DriverManager;
import javax.swing.JOptionPane;

/**
 *
 * @author mdinal
 */
public class KoneksiDB {

    private static Connection conn;

    public static Connection getKoneksi() {
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            String url = "jdbc:mysql://localhost:3306/pv_biodata";
            String user = "root";
            String password = "";
            conn = DriverManager.getConnection(url, user, password);
            JOptionPane.showMessageDialog(null, "Koneksi Berhasil");
        } catch (Exception e) {
            JOptionPane.showMessageDialog(null, "Koneksi Gagal");
        }
        return conn;
    }
}


Test Koneksi dengan membuat jForm baru dan jangan lupa untuk cek koneksi pada tab Services
<img width="524" alt="8" src="https://github.com/user-attachments/assets/775957d4-e272-40d2-91ef-90318e3ff07a">
Keterangan :
• JDBC (Java Data Base Connectivity) untuk menghubungkan Java dan MySQL. • JDBC bertugas menyediakan koneksi ke database, sehingga kita bisa mengakses dan mengelola datanya dari program Java. Ada beberapa istilah yang harus dipahami dalam JDBC: • DriverManager: adalah sebuah class yang mengelola dirver; • Driver: adalah interface yang menangani komunikasi dengan database. • Connection: adalah interface yang menyediakan method untuk menghubungi database; • Statement: adalah interface untuk mengeksekusi query; • ResultSet: adalah interface untuk menampung data hasil query.

Pada Package Form, buatlah jFrame Form dengan nama FormBiodata.
![9](https://github.com/user-attachments/assets/253371b5-715d-4b2f-8168-376d425643df)
Set Properties :
<img width="381" alt="10" src="https://github.com/user-attachments/assets/4b8420f2-f54b-4a79-b38c-9a9d275aab54">
Klik kanan Project → Properties → Run

<img width="509" alt="12" src="https://github.com/user-attachments/assets/89083f44-c8e8-42f8-b230-8af1e8c49fdb">
package form;

Set import :
import config.KoneksiDB;
import javax.swing.JOptionPane;
import java.sql.*;
import javax.swing.table.DefaultTableModel;

Menyiapkan objek yang diperlukan untuk mengelola database
public class FormBiodata extends javax.swing.JFrame {

    private Statement st;
    private ResultSet rs;
    private Connection conn;

    Set pemanggilan method:
    public FormBiodata() {
        initComponents();
        conn = KoneksiDB.getKoneksi();
        tampilData();
    }

    public FormBiodata() {
        initComponents();
        conn = KoneksiDB.getKoneksi();
        tampilData();
    }
    Membuat method tampilData()
    private void tampilData() {
DefaultTableModel kolomtabel = new DefaultTableModel();
kolomtabel.addColumn("No.");
kolomtabel.addColumn("Nama")
kolomtabel.addColumn("NIM");
kolomtabel.addColumn("TTL");
kolomtabel.addColumn("Jenis Kelamin");
kolomtabel.addColumn("Prodi");
kolomtabel.addColumn("No. Telepon");
kolomtabel.addColumn("Alamat");
try {
    int nomor = 1;
    // buat objek statement untuk mengeksekusi query mysql.
    st = conn.createStatement();
    // buat query ke database
    String sql = "Select * FROM tbl_biodata";
    // eksekusi query dan simpan hasilnya di objek ResultSet
    rs = st.executeQuery(sql);
    // tampilkan hasil query menggunakan perulangan while
    while (rs.next()) {
        kolomtabel.addRow(new Object[]{
            ("" + nomor++),
            rs.getString("nama"),
            rs.getString("nim"),
            rs.getString("ttl"),
            rs.getString("jekel"),
            rs.getString("prodi"),
            rs.getString("notelp"),
            rs.getString("alamat"),});
            tabel_biodata.setModel(kolomtabel);
            tabel_biodata.enable(true);
            tfNama.requestFocus();
            }
            } catch (Exception e) {
                JOptionPane.showMessageDialog(null, "Gagal menampilkan data.
                \n" + e.getMessage());
                }
        }
        Keterangan :
• Method executeQuery() akan menghasilkan nilai kembalian berupa objek ResultSet. • Method ini biasanya digunakan untuk mengambil data dari database.
Membuat method clearForm()
// membuat method clear form
private void clearForm() {
tfNama.setText("");
tfNIM.setText("");
tfTTL.setText("");
buttonGroup1.clearSelection();
cmbProdi.setSelectedItem("-- Program Studi --");
tfNomorTelepon.setText("");
taAlamat.setText("");
tfNama.requestFocus();
}

Skrip Button Submit :
<img width="946" alt="13" src="https://github.com/user-attachments/assets/aed5760b-c9e1-42f0-8a9a-98a16242e7bf">
private void btnSubmitActionPerformed(java.awt.event.ActionEvent evt) {
// TODO add your handling code here:
//membuat validasi form kosong
if (tfNama.getText().equals("")
|| tfNIM.getText().equals("")
|| tfTTL.getText().equals("")
|| tfNomorTelepon.getText().equals("")
|| taAlamat.getText().equals("")
|| buttonGroup1.isSelected(null)
|| cmbProdi.getSelectedItem().equals("-- Program Studi --")) {
    JOptionPane.showMessageDialog(this, "Field harap di isi !",
    "Validasi", JOptionPane.ERROR_MESSAGE);
    return;
    }
    try {
        String cekDB = "SELECT * FROM tbl_biodata WHERE nim = '" +
        tfNIM.getText() + "' ";
        rs = st.executeQuery(cekDB);
        if (rs.next()) {
            JOptionPane.showMessageDialog(null, "NIM sudah tersedia !");
            } else {
                st = conn.createStatement();
                String jekel;
                if (rbLaki.isSelected()) {
                    jekel = "Laki - Laki";
                    } else {
                        jekel = "Perempuan";
                        }
                        //aksi simpan data (Submit/Save/Add)
                        String sql = "INSERT INTO tbl_biodata1 VALUES ('"
                        + tfNama.getText()
                        + "', '" + tfNIM.getText()
                        + "', '" + tfTTL.getText()
                        + "', '" + jekel
                        + "', '" + cmbProdi.getSelectedItem()
                        + "', '" + tfNomorTelepon.getText()
                        + "', '" + taAlamat.getText()
                        + "') ";
                        st.executeUpdate(sql);
                        JOptionPane.showMessageDialog(null, "Data berhasil di simpan");
                        tampilData();
                        clearForm();
                        }
                        } catch (Exception e) {
                            JOptionPane.showMessageDialog(this, e.getMessage());
                            }
                        }
 Keterangan : • JDBC (Java Data Base Connectivity) untuk menghubungkan Java dan MySQL. • JDBC bertugas menyediakan koneksi ke database, sehingga kita bisa mengakses dan mengelola datanya dari program Java. Ada beberapa istilah yang harus dipahami dalam JDBC: • DriverManager: adalah sebuah class yang mengelola dirver; • Driver: adalah interface yang menangani komunikasi dengan database. • Connection: adalah interface yang menyediakan method untuk menghubungi database; • Statement: adalah interface untuk mengeksekusi query; • ResultSet: adalah interface untuk menampung data hasil query.
 Klik ganda pada button Delete, dan mmasukkan skrip berikut :
 private void btnDeleteActionPerformed(java.awt.event.ActionEvent evt) {
// TODO add your handling code here:
hapusData();
}
kemudian kita akan membuat method hapusData
private void hapusData() {
    int row = tabel_biodata.getSelectedRow();
    if (row == -1) {
        JOptionPane.showMessageDialog(this, "Pilih baris yang akan di hapus !");
        return;
    }
    int jawab = JOptionPane.showOptionDialog(this,
    "Apakah Anda ingin menghapus data ini?",
    "Konfirmasi",
    JOptionPane.YES_NO_OPTION,
    JOptionPane.QUESTION_MESSAGE, null, null, null);
    if (jawab == JOptionPane.YES_OPTION) {
        try {
            String sql = "DELETE FROM tbl_biodata WHERE nim='" +
            tfNIM.getText() + "' ";
            pst = conn.prepareStatement(sql);
            pst.executeUpdate();
            JOptionPane.showMessageDialog(null, "Data berhasil di hapus");
            clearForm();
            tampilData();
            } catch (Exception e) {
                JOptionPane.showMessageDialog(this, e.getMessage());
                }
                } else {
                    clearForm();
                    tampilData();
                    }
 Klik kanan pada desain table → Events → Mouse → mouseClicked
 <img width="346" alt="14" src="https://github.com/user-attachments/assets/c6bd1bb8-7dfe-4ef3-8545-355b65e87ab7             
 Masukan skrip berikut :
 private void tabel_biodataMouseClicked(java.awt.event.MouseEvent evt) {
 // TODO add your handling code here
 int row = tabel_biodata.getSelectedRow();
if (tabel_biodata.getValueAt(row, 4).equals("Laki-laki")) {
 rbLaki.setSelected(true);
} else if (tabel_biodata.getValueAt(row, 4).equals("Perempuan")) {
 rbPerempuan.setSelected(true);
  }
 tfNama.setText(tabel_biodata.getValueAt(row, 1).toString());
  tfNIM.setText(tabel_biodata.getValueAt(row, 2).toString());
 tfTTL.setText(tabel_biodata.getValueAt(row, 3).toString());
 cmbProdi.setSelectedItem(tabel_biodata.getValueAt(row, 5).toString());
 tfNomorTelepon.setText(tabel_biodata.getValueAt(row, 6).toString());
 taAlamat.setText(tabel_biodata.getValueAt(row, 7).toString());
 }Jalankan aplikasinya
 <img width="778" alt="9" src="https://github.com/user-attachments/assets/a2bd4be4-9ca1-47c9-a2bd-fd8e069f408d">
 Laporan:               
[UTS-PEMOGRAMAN VISUAL.pdf](https://github.com/user-attachments/files/17655565/UTS-PEMOGRAMAN.VISUAL.pdf)
