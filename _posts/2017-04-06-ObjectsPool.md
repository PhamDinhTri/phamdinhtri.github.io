# Design Pattern: Objects Pool

## Giới thiệu

Việc khởi tạo và hủy đối tượng trong game là một việc gây tốn rất nhiều tài nguyên. Ví dụ như trong game bắn máy bay bạn có thể phải tạo hàng ngàn viên đạn trong thời gian ngắn. Để giải quyết vấn đề này ta sử dụng một mẫu thiết kế là objects pool. Objects pool giúp cải thiện hiệu suất và sử dụng bộ nhớ bằng sử dụng lại các đối tượng từ một pool cố định thay vì việc khởi tạo và hủy bỏ chúng.

## Tiền đề bài viết

Trong quá trình phát triển game tôi có sử dụng hiệu ứng sét đánh của bài viết  [Tạo Hiệu Ứng Sét Đánh](https://www.stdio.vn/articles/read/85/tao-hieu-ung-set-danh-cocos2d-x-2xx) thuộc [STDIO](https://www.stdio.vn). Trong game tôi sử dụng nhiều lần và liên tục nên game đã bị crash. Trong quá trình giải quyết thì tôi đã đọc được kĩ thuật objects pool này.

## Đối tượng hướng đến

Bài viết hướng đến những bạn đã có kiến thức về cocos2d-x và mẫu thiết kế.

## Objects Pool

Objects pool là một lớp dùng để lưu trữ lại các đối tượng. Bạn sẽ khởi tạo tối đa số lượng đối tượng có thể được sử dụng và đưa vào pool. Khi đối tượng được sử dụng xong bạn sẽ không hủy nó mà sẽ đưa nó ra khỏi màn hình hiển thị hoặc ẩn đi. Objects pool được sử dụng khi:

Khi bạn cần tạo và hủy đối tượng liên tục
Các đối tượng là tương tự nhau
Các đối tượng có thể tái sử dụng
Hiện thực

Lớp Objects pool cơ bản gồm 2 phương thức: ``` getObject, addObject.```

Objects pool trong cocos2d-x v3 kế thừa từ Vector
```cpp
#pragma once
#include "cocos2d.h"
USING_NS_CC;

template <class T>
class ObjectPool:public Vector<T>
{
private:
	int m_index;
public:
	ObjectPool<T>() : m_index(0)
	{		
	}

	T getObject()
	{
		T ObjIndex;
		if (m_index <this->size())
		{
			ObjIndex =this->at(m_index);
			m_index++;
		}
		else
		{
			ObjIndex = this->at(0);
			m_index = 1;
		}

		return ObjIndex;
	}

	void addObject(T obj)
	{
		this->pushBack(obj);
	}
};
```
Khi bạn khởi tạo số lượng đủ lớn cần sử dụng thì với biến ```m_index``` tăng dần sẽ đảm bảo sẽ giúp bạn lấy ra đối tượng chưa được sử dụng.

Sử dụng objects pool

Khai báo:
```cpp  
ObjectPool<CatActor*> catActorPool;
```
Khởi tạo đối tượng:
```cpp
for (int i = 0; i < 10; i++)
{
	CatActor* cat = CatActor::create();
	catActorPool.addObject(cat);
	addChild(cat);		
}
```
Khi cần dùng đối tượng:
```cpp
CatActor* cat = catActorPool.getObject();
```
## Tham khảo

<http://gameprogrammingpatterns.com/object-pool.html>
