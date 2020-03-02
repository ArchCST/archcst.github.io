title: Java x D23
date: 2020-01-07
updated: 
comments: true
tags:
  - learn
  - java
layout: post
---
{% cq %}
<span style="font-variant: small-caps;">homework</span>
ParkingSystem
{% endcq %}
<!--more-->
# ParkingSystem
用了各种流实现，前后写了五个版本，这里是 `对象流 + 可变数组` 的实现，这个实现相对逻辑更清晰，代码量大大减少了。
PackingDemo 是 main() 主入口，用到了 [Day_21](https://archcst.me/WNCD67/Week04/day_21/) 中写的 Faker 生成随机车牌和随机入场时间。
{% tabs code %}
<!-- tab Ticket 类 -->
```java
import java.io.Serializable;
import java.math.BigDecimal;
import java.text.SimpleDateFormat;

public class Ticket implements Serializable {

    private static final long serialVersionUID = 4463501559751555605L;
    private String plate;
    private String color;
    private long inTime;
    private long outTime;
    private BigDecimal charge;

    // 入场构造器
    public Ticket(String plate, String color, long inTime) {
        this.plate = plate;
        this.color = color;
        this.inTime = inTime;
    }

    public String getPlate() {
        return plate;
    }

    //出场构造器
    public Ticket(String plate) {
        this.plate = plate;
    }

    @Override
    public String toString() {
        StringBuilder re = new StringBuilder();
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");

        String[] lines = new String[9];
        lines[0] = "╭────────── Packing System ───────────╮";
        lines[1] = "│                                     │";
        lines[2] = "│  " + "车 牌 号: " + plate + "                 │";
        lines[3] = "│  " + "颜    色: " + color + "                       │";
        lines[4] = "│  " + "入场时间: " + sdf.format(inTime) + "  │";
        lines[5] = "│  " + "出场时间: " + sdf.format(outTime) + "  │";
        lines[6] = "│  ┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄  │";
        lines[8] = "╰─────────────────────────────────────╯";

        StringBuilder sb = new StringBuilder("│              ");
        for (int i = 0; i < 12 - (""+charge).length(); i++) {
            sb.append(" ");
        }
        sb.append("收费: ").append(charge).append(" 元  │");
        lines[7] = sb.toString();

        for (String line : lines) {
            re.append(line);
            re.append("\n");
        }
        re.append("\n");

        return re.toString();
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        } else if (obj instanceof Ticket) {
            Ticket t = (Ticket) obj;
            return this.plate.equals(t.plate);
        }
        return false;
    }

    // 设置出场时间时计算成员变量 charge
    public void setOutTime(long outTime) {
        BigDecimal inTimeD = new BigDecimal(inTime);
        BigDecimal outTimeD = new BigDecimal(outTime);

        charge = outTimeD.subtract(inTimeD).divide(new BigDecimal("3600000"), BigDecimal.ROUND_CEILING).multiply(new BigDecimal("5"));
        this.outTime = outTime;
    }
}
```
<!-- endtab -->

<!-- tab DaraCenter 类 -->
```java
import java.io.*;
import java.util.ArrayList;
import java.util.List;
import java.util.Random;

public class DataCenter {

    private static DataCenter dataCenter = null;
    private static List<Ticket> pool;
    private static File pFile = new File("src/Projects/ParkingSystem/_pool.db");
    private static File aFile = new File("src/Projects/ParkingSystem/_archive.txt");

    private DataCenter() {
        // 文件为空会在创建ObjectInputStream时报错
        if (pFile.length() == 0) {
            pool = new ArrayList<>();
            writeToPoolDB();
        } else {
            readFromPoolDB();
        }
    }

    public static DataCenter getInstance() {
        if (dataCenter == null) {
            dataCenter = new DataCenter();
        }
        return dataCenter;
    }

    private static void readFromPoolDB() {
        InputStream inputStream = null;
        ObjectInputStream objectInputStream = null;
        try {
            inputStream = new FileInputStream(pFile);
            objectInputStream = new ObjectInputStream(inputStream);
            pool = (ArrayList<Ticket>) objectInputStream.readObject();
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        } finally {
            if (objectInputStream != null) {
                try {
                    objectInputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            if (inputStream != null) {
                try {
                    inputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

    }

    private static void writeToPoolDB() {
        OutputStream outputStream = null;
        ObjectOutputStream objectOutputStream = null;

        try {
            outputStream = new FileOutputStream(pFile);
            objectOutputStream = new ObjectOutputStream(outputStream);
            objectOutputStream.writeObject(pool);
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (objectOutputStream != null) {
                try {
                    objectOutputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            if (outputStream != null) {
                try {
                    outputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    public int findTicketFromPool(Ticket ticket) {
        return pool.indexOf(ticket);
    }

    public Ticket findTicketFromPool(int index) {
        return pool.get(index);
    }

    public void appendToPool(Ticket ticket) {
        pool.add(ticket);
        writeToPoolDB();
    }

    public void archiveTicket(Ticket ticket) {
        pool.remove(ticket);
        writeToPoolDB();
        Writer writer = null;
        BufferedWriter bufferedWriter = null;
        try {
            writer = new FileWriter(aFile, true);
            bufferedWriter = new BufferedWriter(writer);
            bufferedWriter.write(ticket.toString());
            bufferedWriter.flush();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (bufferedWriter != null) {
                try {
                    bufferedWriter.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            if (writer != null) {
                try {
                    writer.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    public String randomPlate() {
        Random r = new Random();
        if (!pool.isEmpty()) {
            return pool.get(r.nextInt(pool.size())).getPlate();
        }
        return "";
    }

    public void listPool() {
        for (Ticket ticket : pool) {
            System.out.println(ticket);
        }
    }
}
```
<!-- endtab -->

<!-- tab TicketService 类 -->
```java
import java.util.Date;

public class TicketService {

    private DataCenter dataCenter = DataCenter.getInstance();

    public void carIn(String plate, String color, long inTime) {
        Ticket ticket = new Ticket(plate, color, inTime);

        if (dataCenter.findTicketFromPool(ticket) != -1) {
            System.out.println("重复入场，已报警！");
        } else {
            dataCenter.appendToPool(ticket);
        }
    }

    public void carOut(String plate) {
        Ticket ticket = new Ticket(plate);

        int index = dataCenter.findTicketFromPool(ticket);
        if (index == -1) {
            System.out.println("找不到该车！请联系管理员！");
        } else {
            ticket = dataCenter.findTicketFromPool(index);
            ticket.setOutTime(new Date().getTime());
            System.out.println(ticket);
            dataCenter.archiveTicket(ticket);
        }
    }

    public void listPool() {
        dataCenter.listPool();
    }

    public String randomPlateInPool(){
        return dataCenter.randomPlate();
    }
}
```
<!-- endtab -->

<!-- tab PackingDemo 类 -->
```java
import playground.Faker;
import java.util.Date;
import java.util.Scanner;

public class PackingDemo {
    public static void main(String[] args) {
        TicketService ticketService = new TicketService();
        Scanner sc = new Scanner(System.in);
        Faker faker = new Faker();
        while (true) {
            String plate;
            String color;
            System.out.print("1.随机入场 2.手动入场 3.随机出场 4.手动出场 5.退出：");
            char c = sc.next().charAt(0);
            switch (c) {
                case '0':
                    ticketService.listPool();
                    break;
                case '1':
                    plate = faker.plate();
                    color = faker.color();
                    System.out.println("入场车：" + plate + " " + color + "色");
                    ticketService.carIn(plate, color, faker.timeBefore(0,1,30));
                    break;
                case '2':
                    System.out.print("车牌号：");
                    plate = sc.next();
                    System.out.print("颜  色：");
                    color = sc.next();
                    ticketService.carIn(plate, color, new Date().getTime());
                    break;

                case '3':
                    plate = ticketService.randomPlateInPool();
                    if (plate.equals("")) {
                        System.out.println("停车场空了！");
                    } else {
                        ticketService.carOut(plate);
                    }
                    break;
                case '4':
                    System.out.print("出场车牌号：");
                    plate = sc.next();
                    ticketService.carOut(plate);
                    break;
                case '5':
                    System.out.println("撒哟啦啦~");
                    return;
                default:
                    System.out.println("你的输入有误，请重试");
                    break;
            }

        }

    }
}
```
<!-- endtab -->
{% endtabs %}