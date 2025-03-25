## User Table

## Description

This table contains information about users.

## GORM Model

```go
type User struct {
	ID        uint           	`gorm:"primaryKey"`
	Email     string         	`gorm:"unique"`
	Username  string         	`gorm:"unique"`
	Password  string					`gorm:"not null"`
	CreatedAt time.Time      	`gorm:"autoCreateTime"`
	UpdatedAt time.Time      	`gorm:"autoUpdateTime"`
	DeletedAt gorm.DeletedAt 	`gorm:"index"`
}
```
