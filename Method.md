# Сбор данных:  
```powershell
# Логирование всех активных подключений (аналог netstat)
Get-NetTCPConnection | Where-Object { $_.State -eq "Established" } | Export-Csv connections.csv

# Захват трафика через пакетный снэпшот (требует прав администратора)
New-NetEventSession -Name "TrafficCapture" -CaptureMode SaveToFile -LocalFilePath "C:\traffic.etl"
Start-NetEventSession -Name "TrafficCapture"
# Через 24 часа останавливаем и анализируем:
Stop-NetEventSession -Name "TrafficCapture"
```

# Анализ:  
```powershell
# Группировка по IP и портам (для формирования правил)
Import-Csv connections.csv | Group-Object RemoteAddress, RemotePort | Sort-Object Count -Descending
```

---

# Nmap (для инвентаризации сервисов)  
```
Перед настройкой правил полезно провести инвентаризацию:  
- Какие порты открыты?  
- Какие сервисы работают?  
```
**Пример сканирования**:  
```bash
nmap -sV -p 1-65535 192.168.1.0/24 -oX scan_results.xml
```

---

# SolarWinds Network Performance Monitor** (для корпоративного использования)  
```
- Автоматическое построение карты сети.  
- Выявление аномальных подключений.  
- Генерация отчетов для ACL.  
```

---
