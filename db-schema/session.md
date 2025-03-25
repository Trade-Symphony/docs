## Session Table

## Description

This table contains information about sessions.

## GORM Model

```go
type Session struct {
	ID          string         	`gorm:"primaryKey"`
	UserID      uint            `gorm:"index"`
	Expiry      time.Time       `gorm:"not null"`
	UserAgent   string          `gorm:"not null"`
	IP          string          `gorm:"not null"`
	CreatedAt   time.Time      	`gorm:"autoCreateTime"`
	UpdatedAt   time.Time      	`gorm:"autoUpdateTime"`
	DeletedAt   gorm.DeletedAt 	`gorm:"index"`
}
```
