# การทดลอง ESP32 FreeRTOS 
##  ฟังก์ชัน vTaskSuspend() และ vTaskResume()

จากการทดลองที่แล้ว `vTaskDelete(MySecondTaskHandle);` จะลบ `MySecondTaskHandle` ออกจาก task  list และไม่สามารถเรียกกลับมาได้ใหม่ เรามีวิธีการที่จะหยุดและรันต่อ โดยใช้คำสั่ง `vTaskSuspend()` และ `vTaskResume()`

1. แก่ไข Code จากโปรแกรมที่แล้ว โดยการเพิ่ม task เข้าไปอีก 1 task (ดู comment ใน code)

```c
...

void My_First_Task(void * arg)
{
	uint16_t i = 0;
	while(1)
	{
		printf("Hello My First Task %d\n",i);
		vTaskDelay(1000/portTICK_PERIOD_MS);
		i++;

		if(i == 5)
		{
			vTaskSuspend(MySecondTaskHandle);
			printf("Second Task suspended\n");
		}

		if(i == 10)
		{
			vTaskResume(MySecondTaskHandle);
			printf("Second Task Resumed\n");
		}

		if(i == 15)
		{
			vTaskDelete(MySecondTaskHandle);
			printf("Second Task deleted\n");
		}

		if(i == 20)
		{
			printf("MyFirstTaskHandle will suspend itself\n");
			vTaskSuspend(NULL);
		}
	}
}
...
```
โดยปกติเราจะใส่ชื่อ task handle เป็นอาร์กิวเมนต์สำหรับฟังก์ชัน `vTaskDelete()`, `vTaskSuspend()` และ `vTaskResume()`  แต่การใส่อาร์กิวเมนต์เป็น `NULL` เช่น  `vTaskSuspend(NULL);` จะหมายถึงการกระทำกับ task ผู้ออกคำสั่งเอง 

4. รันและบันทึกผลจากโปรแกรมข้างบน วิเคราะห์ผลที่ได้ว่าเป็นอย่างไร

![image](https://github.com/user-attachments/assets/6f26f8d7-2a20-4b63-ace7-040519e5c590)

แสดงผล Hello My First Task และ Hello My Second Task ตามลำดับ ถ้าเงื่อนไขเป็น 5 จะหยุด run Hello My Second Task ถ้าเงื่อนไขเป็น 10 จะเรียก run Hello My Second Task ใหม่ ถ้าเงื่อนไขเป็น 15 จะเรียกทำการ deleted Hello My Second Task ออก
ถ้าเงื่อนไขเป็น 20 จะหยุดทั้ง 2

## [>> ต่อไป >>](./ESP32-FreeRTOS-Labsheet-6.md) 
