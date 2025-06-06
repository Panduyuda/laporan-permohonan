<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Laporan Permohonan Bank Sampah</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 text-gray-800">
  <div class="max-w-4xl mx-auto p-4">
    <h1 class="text-2xl font-bold mb-4">Formulir Permohonan Bantuan</h1>
    <form id="permohonanForm" class="bg-white shadow-md rounded p-4 mb-8">
      <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
        <input name="tanggal" type="date" class="border p-2 rounded" placeholder="Tanggal Permohonan" required />
        <input name="noSurat" type="text" class="border p-2 rounded" placeholder="No Surat" required />
        <input name="namaPemohon" type="text" class="border p-2 rounded" placeholder="Nama Pemohon/Instansi" required />
        <input name="alamat" type="text" class="border p-2 rounded" placeholder="Alamat" required />
        <input name="keterangan" type="text" class="border p-2 rounded" placeholder="Keterangan Permohonan" required />
        <select name="realisasi" class="border p-2 rounded">
          <option value="">Status Realisasi</option>
          <option value="Sudah">Sudah</option>
          <option value="Belum">Belum</option>
          <option value="TL">TL</option>
        </select>
      </div>
      <button type="submit" class="mt-4 bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded">Kirim</button>
    </form>

    <h2 class="text-xl font-semibold mb-2">Data Permohonan</h2>
    <div class="overflow-auto">
      <table class="min-w-full bg-white">
        <thead>
          <tr>
            <th class="py-2 px-4 border">Tanggal</th>
            <th class="py-2 px-4 border">No Surat</th>
            <th class="py-2 px-4 border">Pemohon</th>
            <th class="py-2 px-4 border">Alamat</th>
            <th class="py-2 px-4 border">Keterangan</th>
            <th class="py-2 px-4 border">Realisasi</th>
          </tr>
        </thead>
        <tbody id="dataTable">
          <!-- Data akan dimuat di sini -->
        </tbody>
      </table>
    </div>
  </div>

  <script>
    // Ganti URL berikut dengan Google Apps Script Web App kamu
    const scriptURL = 'https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec';
    const form = document.getElementById('permohonanForm');

    form.addEventListener('submit', e => {
      e.preventDefault();
      const formData = new FormData(form);
      fetch(scriptURL, { method: 'POST', body: formData })
        .then(response => {
          alert('Permohonan berhasil dikirim!');
          form.reset();
          loadData();
        })
        .catch(error => alert('Gagal mengirim data.'));
    });

    async function loadData() {
      const sheetURL = 'https://docs.google.com/spreadsheets/d/e/YOUR_SHEET_PUBLIC_CSV/pub?output=csv';
      const res = await fetch(sheetURL);
      const text = await res.text();
      const rows = text.split('\n').slice(1);
      const tbody = document.getElementById('dataTable');
      tbody.innerHTML = '';
      rows.forEach(row => {
        const cols = row.split(',');
        const tr = document.createElement('tr');
        cols.forEach(col => {
          const td = document.createElement('td');
          td.textContent = col;
          td.className = 'border px-4 py-2';
          tr.appendChild(td);
        });
        tbody.appendChild(tr);
      });
    }

    loadData();
  </script>
</body>
</html>
