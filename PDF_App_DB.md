- B1: Để giám sát CPU vs RAM phần này ta sẽ install chung cho các 3 server là node exporter 
      mục đích của nó là monitor cpu vs ram để đẩy đến promotheus để lưu trữ 
  - B1.1: Install node exporter thông qa github :
  
          cd /opt/
          wget https://github.com/prometheus/node_exporter/releases/download/v0.18.1/node_exporter-0.18.1.linux-amd64.tar.gz
          tar -xzvf node_exporter-0.18.1.linux-amd64.tar.gz
          mv node_exporter-0.18.1.linux-amd64/node_exporter /usr/local/bin/

          useradd --no-create-home --shell /bin/false node_exporter
          
          nano /etc/systemd/system/node_exporter.service
          
          => Paste nó vào node_exporter.service :
            [Unit]
            Description=Node Exporter
            Wants=network-online.target
            After=network-online.target

            [Service]
            User=node_exporter
            Group=node_exporter
            Type=simple
            ExecStart=/usr/local/bin/node_exporter

            [Install]
            WantedBy=default.target
          
          => Save node_exporter.service
          
  - B1.2: Start node exporter :
  
          systemctl daemon-reload
          systemctl enable node_exporter
          systemctl start node_exporter
          systemctl status node_exporter

          ● node_exporter.service - Node Exporter
          Loaded: loaded (/etc/systemd/system/node_exporter.service; enabled; vendor preset: disabled)
          Active: active (running) since Wed 2019-08-21 22:54:01 +07; 7s ago
          Main PID: 5573 (node_exporter)
          CGroup: /system.slice/node_exporter.service
          └─5573 /usr/local/bin/node_exporter
          
  - B1.3: Mở port cho exporter như 9090 9100 thông qua ufw:
    - B1.3.1: sudo ufw allow 9100
    - B1.3.2: sudo ufw allow 9090
    
  - B1.4: Nếu test có thể truy cập vào ip://9100 để xem kết quả có thành công hay không
  
- B2: Cai đặt promothues để query các số liệu lấy dừ exporter 
  - B2.1: Tạo Prometheus system group https://gist.github.com/luandevpro/99138e7888b5415ef9ebd044c3902131#file-terminal
  - B2.2: Tạo data & configs  thư mục cho Prometheus https://gist.github.com/luandevpro/891a5fd4639af2ea6044009e56f95de9#file-terminal
  - B2.3: Download Prometheus https://gist.github.com/luandevpro/3fea24865f9b4d25db54f6b3ad14401b#file-terminal
  - B2.4: Config Promotheus trên server ubuntu 
    - https://gist.github.com/luandevpro/3d11cb86ca5dbe3638efb80d54626323#file-aterminal
    - https://gist.github.com/luandevpro/017e733b778d4ed994438ceab9754e77#file-terminal
    - https://gist.github.com/luandevpro/61c606993b8f9fccb3126facf073f374#file-terminal
    - https://gist.github.com/luandevpro/e9e6903bb4cd6a30208654da5c939076#file-terminal
    - Chú ý thây đôi localhost:9090 ở trên bằng ip vs port của node_exporters vừa tạo ra ,bạn có thể truy cập vào địa chỉ ip:9090 để xem kết quả server promotheus như thế nào nhé .
  - B2.5: 
