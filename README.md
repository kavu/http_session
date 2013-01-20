В данном пакете предусмотрено 2 режима работы с веб-сессиями:
- классический файловый режим
- режим работы с ОЗУ

Установка пакета
================
Для установки пакета наберите в консоли следущую команду:

go get github.com/Trusow/http_session

Инициализация сессий
====================
Для инициализиции файлового режима или режима работы с ОЗУ необходима следующая функция:

func Init(name string, limit int32, time int64, file ... string) (*session, error)

Где переменная name задает имена файлам cookie при установке сессии, переменная limit отвечает за максимальное количество сессий, переменная time отвечает за время хранения сессии. Переменная file является необязательной. Если она отсутствует, то устанавливается режим работы с ОЗУ. Если она присутствует, то соответственно устанавливается файловый режим. Переменная file при ее установке должна содержать путь к директории веб сессий.
Функция Init возвращает 2 значения: первое - вновь созданную структуру веб-сессий, вторую - ошибку при создании структуры.

Установка значений сессий
=========================
Для установки значений сессий используется следующая функция:

func (s *session) Set (name string, value string, w http.ResponseWriter, r* http.Request)

Где name является именем значения, value - непосредственно значением (если value является "", то из памяти или файла удаляется не только значение, но и имя). w является ответом сервера, r -запросом к серверу.

Чтение значений сессий
======================
Для чтения значений сессий используется следующая функция:

func (s *session) Get (name string, r *http.Request) string

Где name является именем значения, r - является запросом к серверу. В случае отсутствия значения возвращается ""

Удаление сессий
===============
Для удаления сессии используется следующая функция:

func (s *session) Remove(r *http.Request)

Где r является запросом к серверу

Пример использования
====================
package main

import(

  "net/http"
  
  "github.com/Trusow/http_session"
  
)
var video,_ = http_session.Init("video", 10, 10, "./video/")

func requestHandler(w http.ResponseWriter, r *http.Request){

  video.Set("name","play", w, r)
  
  w.Header().Add("Content-Type", "text/html;charset=utf-8")
  
  w.Header().Add("Content-Language", "ru")
  
  w.Write([]byte(video.Get("name",r)))
  
}

func qweHandler(w http.ResponseWriter, r *http.Request){

}
func main(){

  http.HandleFunc("/", requestHandler)
  
  http.HandleFunc("/favicon.ico", qweHandler)
  
  http.ListenAndServe(":8080", nil)
  
}


Ошибки
======
Обо всех найденных ошибках просьба сообщать по этому ящику serge-ruso@ya.ru
