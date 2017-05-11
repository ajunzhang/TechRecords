#### OpenFile
> go语言中的OpenFile函数第一个参数是文件名称，第二个是标记读写等，第三个是权限值

>OpenFile源码

	// OpenFile is the generalized open call; most users will use Open
	// or Create instead. It opens the named file with specified flag
	// (O_RDONLY etc.) and perm, (0666 etc.) if applicable. If successful,
	// methods on the returned File can be used for I/O.
	// If there is an error, it will be of type *PathError.
	func OpenFile(name string, flag int, perm FileMode) (*File, error) {
		if name == "" {
			return nil, &PathError{"open", name, syscall.ENOENT}
		}
		r, errf := openFile(name, flag, perm)
		if errf == nil {
			return r, nil
		}
		r, errd := openDir(name)
		if errd == nil {
			if flag&O_WRONLY != 0 || flag&O_RDWR != 0 {
				r.Close()
				return nil, &PathError{"open", name, syscall.EISDIR}
			}
			return r, nil
		}
		return nil, &PathError{"open", name, errf}
	}

Flags常量枚举如下：
	
	// Flags to OpenFile wrapping those of the underlying system. Not all
	// flags may be implemented on a given system.
	const (
		O_RDONLY int = syscall.O_RDONLY // open the file read-only.
		O_WRONLY int = syscall.O_WRONLY // open the file write-only.
		O_RDWR   int = syscall.O_RDWR   // open the file read-write.
		O_APPEND int = syscall.O_APPEND // append data to the file when writing.
		O_CREATE int = syscall.O_CREAT  // create a new file if none exists.
		O_EXCL   int = syscall.O_EXCL   // used with O_CREATE, file must not exist
		O_SYNC   int = syscall.O_SYNC   // open for synchronous I/O.
		O_TRUNC  int = syscall.O_TRUNC  // if possible, truncate file when opened.
	)

权限码一般给666