CPP_OPENSSL_OSX := -L/usr/local/opt/openssl/lib -I/usr/local/opt/openssl/include
OBJS = Extensions.o Group.o Networking.o Hub.o Node.o WebSocket.o HTTPSocket.o Socket.o Epoll.o Room.o

TARGET = ../libuWS.so

$(TARGET): $(OBJS)
	g++ -std=c++11 -shared -g -o $(TARGET) $(OBJS) $(CPP_OPENSSL_OSX) -lssl -lcrypto -lz -luv -luuid

Extensions.o: Extensions.cpp Extensions.h
	g++ -std=c++11 -Wall -c -fPIC -g Extensions.cpp

Networking.o: Networking.cpp Networking.h
	g++ -std=c++11 -Wall -c -fPIC -g Networking.cpp 

Epoll.o: Epoll.cpp Epoll.h
	g++ -std=c++11 -Wall -c -fPIC -g Epoll.cpp 

Socket.o: Socket.cpp Socket.h Networking.o
	g++ -std=c++11 -Wall -c -fPIC -g Socket.cpp 

HTTPSocket.o: HTTPSocket.cpp HTTPSocket.h Networking.o
	g++ -std=c++11 -Wall -c -fPIC -g HTTPSocket.cpp 

WebSocket.o: WebSocket.cpp WebSocket.h Networking.o
	g++ -std=c++11 -Wall -c -fPIC -g WebSocket.cpp 

Node.o: Node.cpp Node.h Socket.o
	g++ -std=c++11 -Wall -c -fPIC -g Node.cpp 

Room.o: Room.cpp Room.h WebSocket.o
	g++ -std=c++11 -Wall -c -fPIC -g Room.cpp 

Group.o: Group.cpp Group.h WebSocket.o HTTPSocket.o Extensions.o
	g++ -std=c++11 -Wall -c -fPIC -g Group.cpp 

Hub.o: Hub.cpp Hub.h Group.o Node.o
	g++ -std=c++11 -Wall -c -fPIC -g Hub.cpp 

.PHONY: clean
clean:
	rm *.o
	rm $(TARGET)

.PHONY: install
install:
	$(eval PREFIX ?= /usr)
	if [ -d "/usr/lib64" ]; then mkdir -p $(PREFIX)/lib64 && cp ../libuWS.so $(PREFIX)/lib64/; else mkdir -p $(PREFIX)/lib && cp ../libuWS.so $(PREFIX)/lib/; fi
	mkdir -p $(PREFIX)/include/uWS
	cp ../src/*.h $(PREFIX)/include/uWS/