import type {Metadata} from 'next';
import { Inter } from 'next/font/google';
import './globals.css'; // Global styles
import './custom.css';

const inter = Inter({
  subsets: ['latin'],
  variable: '--font-sans',
});

export const metadata: Metadata = {
  title: 'Nexus Connect',
  description: 'A fully functional real-time messenger with channels and direct messaging.',
};

export default function RootLayout({children}: {children: React.ReactNode}) {
  return (
    <html lang="en" className={`${inter.variable}`}>
      <body className="font-sans antialiased text-[#E4E4E7] bg-[#0F0F12]" suppressHydrationWarning>
        {children}
      </body>
    </html>
  );
}
