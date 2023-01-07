# epoll_wait-kernel-module

## trace

glibc
https://codebrowser.dev/glibc/glibc/sysdeps/unix/sysv/linux/epoll_wait.c.html
```c
#define __NR_epoll_wait 232

...

int
epoll_wait (int epfd, struct epoll_event *events, int maxevents, int timeout)
{
#ifdef __NR_epoll_wait
  return SYSCALL_CANCEL (epoll_wait, epfd, events, maxevents, timeout);
#else
  return epoll_pwait (epfd, events, maxevents, timeout, NULL);
#endif
}
```

syscall 232
https://filippo.io/linux-syscall-table/

fs/eventpoll.c
https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/fs/eventpoll.c
```c
SYSCALL_DEFINE4(epoll_wait, int, epfd, struct epoll_event __user *, events,
		int, maxevents, int, timeout)
{
	struct timespec64 to;

	return do_epoll_wait(epfd, events, maxevents,
			     ep_timeout_to_timespec(&to, timeout));
}
```
