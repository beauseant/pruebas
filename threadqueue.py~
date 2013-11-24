import Queue
import threading
import urllib2
import time



class ThreadUrl(threading.Thread):
	"""Threaded Url Grab"""
	__idth = 0

	def __init__(self, queue, idth):		
		self.__idth = idth
		threading.Thread.__init__(self)
		self.queue = queue

	def run(self):
		while True:
			#grabs host from queue
			host = self.queue.get()
			print 'el hilo %s ha cogido el trabajo %s' % (self.__idth, host)
			#grabs urls of hosts and prints first 1024 bytes of page
			url = urllib2.urlopen(host)
			#print url.read(4)

			#signals to queue job is done
			self.queue.task_done()



def main( queue, hosts):

	#spawn a pool of threads, and pass them queue instance 
	for i in range(3):
		t = ThreadUrl(queue, i)
		t.setDaemon(True)
		t.start()

	#populate queue with data   
	for host in hosts:
		queue.put(host)

	#wait on the queue until everything has been processed     
	queue.join()


if __name__ == "__main__":

	hosts = ["http://yahoo.com", "http://google.com", "http://amazon.com",
"http://ibm.com", "http://apple.com"]

	queue = Queue.Queue()

	start = time.time()
	main( queue, hosts)
	print "Elapsed Time: %s" % (time.time() - start)
