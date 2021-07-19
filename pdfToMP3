import time
import PyPDF2
from gtts import gTTS
from playsound import playsound
import multiprocessing


	
class cleanPDF:

#choose name for pdf/mp3 file 
	def __init__(self, filename):
		self.filepathPDF = '/Users/suga/PythonProjects/Audiobooks/' + filename + '.pdf'
		self.open = open(self.filepathPDF, 'rb')
		self.reader = PyPDF2.PdfFileReader(self.open, strict=False)

	#read pdf file
	def pdfToDict(self): 
		self.pages = self.reader.getNumPages()
		pageDict = {}
		for page in range(self.pages):
			self.page = self.reader.getPage(page)
			self.text = self.page.extractText()
			self.cleantext = self.text.replace(f'Napoleon Hill   |   {page+1}','').replace('Think and Grow Rich','')
			pageDict[f'{page}'] = self.cleantext
		
		return pageDict

class audioBook:

	def __init__(self, filename, ttconvert):
		self.text = str(ttconvert)
		self.filepathMP3 = f'/Users/suga/PythonProjects/Audiobooks/{filename}_audio/{filename}.mp3'
		self.filename = filename

	def ttmp3(self):
		tts = gTTS(text=self.text, lang='en', tld='co.za', slow=False)
		tts.save(self.filepathMP3)
		return 

	def playSnippet(self): 
		p = multiprocessing.Process(target=playsound, args=(self.filepathMP3,))
		p.start()
		print(f'{self.filename} download complete, playing snippet...')
		time.sleep(15)
		p.terminate()
		return


if __name__ == '__main__':

	filename = 'thinkrich'

	cl = cleanPDF(filename)
	clDict = cl.pdfToDict()

	start = 20
	end = cl.pages
	step = 5

	numeros = [(i,i+step) for i in range(start, end, step)]
	
	chunklist = []
	for n1, n2 in numeros:
		numrange = range(n1, n2)
		numlist = list(numrange)
		chunklist.append([n for n in numlist])
	
	printlist = []
	for chunk in chunklist:
		textout = ''
		for page in chunk:
			textout += clDict[f'{page-1}']
		printlist.append(textout)
	
	print(f'{len(printlist)} chunks to download...')

	for count, batch in enumerate(printlist):
		if count == 0:
			fn = f'{filename}'
		else:
			fn = f'{filename}_{count}'
		
		ab = audioBook(fn,batch)
		ab.ttmp3()
		ab.playSnippet()

	print(filename,'- audiobook download complete')
