#-*- coding: utf-8 -*-
import socket

def main():
    # 创建套接字，AF_INET表示ipv4，SOCK_DGRAM表示udp传输
    udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    
    # 绑定服务器地址和端口
    server_addr = ("192.168.27.128",9999)
    # socket绑定到一个ip和端口号的二元组
    udp_socket.bind(server_addr)
    
    while True:
	#先接受文件名名
	filename,client_addr=udp_socket.recvfrom(1024)

        # 接收数据
        recv_data, client_addr = udp_socket.recvfrom(1024)
        
        # 打印接收到的数据
        print("%s" % recv_data.decode("gbk"))
        
	#创建一个与发送方相同的文件名,参数w表示若文件不存在创建一个新的文件；如果文件已经存在，则会清空文件内容，并从头开始写入数据。
	with open(filename,'w')as file:
	
	#把接收到的文件内容写到新建的文件中
	     file.write(recv_data)

        # 向发送发送收到的文件内容信息
        send_data = "%s" % recv_data.decode("utf-8")
        udp_socket.sendto(send_data.encode("utf-8"), client_addr)

    # 关闭套接字
    udp_socket.close()

if __name__ == "__main__":
    main()

